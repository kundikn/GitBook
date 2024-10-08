# Настройка DMVPN 

Будем настраивать **DMVPN** на *4 маршрутизаторах* с протоколом динамической маршрутизации **EIGRP**.

**Конфигурация для Hub роутера:**

```
interface Tunnel1
 interface Tunnel1
 ip address 192.168.255.1 255.255.255.0
 no ip redirects - эта команда отключает отправку ICMP Redirect messages на указанном интерфейсе
 ip hold-time eigrp 1 35
 no ip next-hop-self eigrp 1
 ip nhrp map multicast dynamic - Hub-маршрутизатор будет автоматически добавлять соответствия между адресами spoke-маршрутизаторов
 ip nhrp network-id 1
 ip nhrp shortcut
 no ip split-horizon eigrp 1
 tunnel source 10.10.10.1
 tunnel mode gre multipoint - включение mGRE-туннеля
 tunnel key 100 - задание ключа, который идентифицирует туннель
```

```
router eigrp 1
 network 192.168.101.0
 network 192.168.255.0
 no auto-summary
```

**Конифгурация одного из споуков, их можно добавлять сколько угодно, лишь менять ip:**

```
interface Tunnel1
 ip address 192.168.255.4 255.255.255.0
 no ip redirects - эта команда отключает отправку ICMP Redirect messages на указанном интерфейсе
 ip nhrp map multicast 10.10.10.1 - адрес внешнего физического интерфейса hub-маршрутизатора указывается как получатель multicast-пакетов от локального маршрутизатора
 ip nhrp map 192.168.255.1 10.10.10.1 - статическое соответствие между адресом mGRE-туннеля и физическим адресом hub-маршрутизатора
 ip nhrp network-id 1
 ip nhrp nhs 192.168.255.1 - адрес туннельного интерфейса hub-маршрутизатора указывается как next-hop-сервер
 ip nhrp shortcut - эта функция позволяет маршрутизаторам напрямую обмениваться трафиком через туннели, минуя хаб, если они уже знают IP-адрес назначения
 tunnel source 10.10.13.1
 tunnel mode gre multipoint - включение mGRE-туннеля
 tunnel key 100 - задание ключа, который идентифицирует туннель
```

```
router eigrp 1
 network 192.168.104.0
 network 192.168.255.0
 no auto-summary
```

# Настройка EIGRP между spoke

Сейчас самое интересное, надо чтобы у нас был **EIGRP**, *вообще для всех протоколов динамической маршрутизации свои нюансы*, сейчас конкретно для **EIGRP**.
Если не сделать всех конкретных действий, то не будут тунелей между споками. 

**На hub-маршрутизаторе необходимо отключить правило расщепления горизонта (split horizon), иначе EIGRP не будет анонсировать маршруты, выученные через mGRE-интерфейс назад в этот же интерфейс**

```
Router(config-if)# no ip split-horizon eigrp 1
```

**По умолчанию EIGRP будет подставлять IP-адрес hub-маршрутизатора в качестве next-hop для маршрутов которые он анонсирует, даже когда анонсирует маршруты назад через тот же интерфейс, на котором они были выучены. Для сети DMVPN необходимо чтобы EIGRP использовал в качестве next-hop адреса spoke-маршрутизаторов. Поэтому на hub-маршрутизаторе необходимо отключить это правило**

```
Router(config-if)# no ip next-hop-self eigrp 1
```

**При использовании DMVPN в больших сетях время сходимости сети может увеличиваться. Для того чтобы избежать возможных проблем с маршрутизацией, на маршрутизаторах необходимо изменить hold time (по умолчанию 15 секунд)**

```
Router(config-if)#ip hold-time eigrp 1 35
```

------------
