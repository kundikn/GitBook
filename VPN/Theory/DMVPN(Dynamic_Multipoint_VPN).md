# DMVPN (Dynamic Multipoint VPN) [СТАТЬЯ](https://habr.com/ru/articles/84738/)

**DMVPN** – вещь сугубо цисковская и открытых адекватных аналогов пока не имеет. GRE over IPsec решает проблему динамической маршрутизации и шифрования данных, но не маштабируемости.

## Суть DMVPN

Суть такая: выбирается центральная точка Hub (или несколько). Она будет сервером, к которому будут подключаться клиенты (Spoke) и получать всю необходимую информацию. При этом:

1. Данные будут зашифрованы IPSec
2. Клиенты могут передавать трафик непосредственно друг другу в обход центрального узла
3. Только на центральном узле необходим статический публичный IP-адрес. Удалённые узлы могут иметь динамический адрес и находиться даже за NATом, используя адреса из частных диапазонов (**Технология NAT Traversal** ). Но при этом возникают ограничения по части динамических туннелей.

Это всё средоточие мощи GRE и IPSec, сдобренное NHRP и IGP.

-------

## Как же это работает и практика

**DMVPN** позволяет динамически изучать адреса удалённых точек, который желают подключиться к основной. Хаб (центральный узел) здесь выступает как сервер (**NHS – Next-Hop Server**), а все удалённые узлы будут клиентами (**NHC – Next-Hop Client**).

### Настройка тунелей

**Конфигурация хаба**

```bash
interface Tunnel0
ip address 172.16.254.1 255.255.255.0 – IP-адрес из нужного диапазона
ip nhrp map multicast dynamic – динамическое изучение данных NHRP от клиентов. Поскольку клиентов у нас множество и они могут быть с динамическими адресами, на хабе нельзя задать явное соответствие внутренних и внешних адресов
ip nhrp network-id 1 – Определяем Network ID – просто идентификатор, который необязательно должен быть одинаковым на всех узлах DMVPN (похож на OSPF Router-ID)
tunnel source FastEthernet0/1.6 – наследие GRE – привязка к физическому интерфейсу
tunnel mode gre multipoint – туннельный интерфейс будет использовать GRE в multipoint-режиме. Интерфейс поддерживает несколько туннельных соединений через один и тот же туннельный интерфейс. Это означает, что на одном физическом интерфейсе можно устанавливать туннели с несколькими удалёнными узлами, и трафик от одного узла может быть направлен к другим узлам напрямую, без необходимости идти через центральный узел
```

**Конфигурация филиала**

```
ip address 172.16.254.2 255.255.255.0 – IP-адрес из нужного диапазона
ip nhrp map 172.16.254.1 198.51.100.2 – Статическое соотношение внутреннего и внешнего адресов хаба
ip nhrp map multicast 198.51.100.2 - мультикастовый трафик должен получать хаб
ip nhrp network-id 1 – Network ID. Не обязательно должен совпадать с таким же на хабе
ip nhrp nhs 172.16.254.1 – Статически настроенный адрес NHRP сервера – хаба. Именно поэтому в центре нам нужен статический публичный адрес
ip nhrp registration no-unique – если адрес в филиалах выдаётся динамически, эта команда обязательна
tunnel source FastEthernet0/0 – привязка к физическому интерфейсу
tunnel mode gre multipoint – указываем, что тип туннеля mGRE – это позволит создавать динамически туннели не только до хаба, но и до других филиалов
```

То есть связность уже обеспечена, но работать филиалы пока не могут – не настроена маршрутизация.

--------

### Настройка динамической маршрутизации

> **Тут для каждого протокола свои всплывают тонкости**

Давайте рассмотрим процесс настройки **OSPF**, для примера.

Поскольку мы имеем широковещательную L2 сеть на туннельных интерфейсах, указываем явно тип сети Broadcast на туннельных интерфейсах на всех узлах:

~~~bash
ip ospf network broadcast
~~~

Кроме того в такой сети должен выбираться DR (Designated Router). Логично, чтобы им стал хаб. Всем Spoke-маршрутизаторам запрещаем участие в выборах DR:

~~~
ip ospf priority 0
~~~

Ну и, естественно, определяем анонсируемые сети:

~~~
router ospf 1
network 172.16.0.0 0.0.255.255 area 0
~~~

*Тут важно понимать, что несмотря на то, что общение между филиалами осуществляется в обход центрального узла, хаб однако несёт тут жизненно важную вспомогательную функцию и без него ничего работать не будет: он предоставляет клиентам таблицу NHRP, а также анонсирует все маршруты – филиалы распространяют маршрутную информацию не непосредственно друг другу, а через хаб.*

--------
