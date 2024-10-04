# Настройка OSPF

**Настройка ospf на маршрутизаторе**

```
router ospf 1
	network 192.168.2.0 0.0.0.255 area 0
	network 10.0.0.0 0.0.0.3 area 0
	network 10.0.0.4 0.0.0.3 area 0
	routerid 1.1.1.1
```

**Посмотреть метрику интерфейса**

```
show ip ospf interface fastEthernet 0/0
```

**Поменять метрику у интерфейса (делает в настройке интерфейса)**

```
R2(config-if)#ip ospf cost 20
```

**Объединяем области, не подключеные физически к Area 0. Если есть 3 зоны, и зона 2 не соеденена с 0, то в первой зоне нужно настроить виртуальный линк между маршрутизаторами с двух сторон. ** *Также при помощи virtual-link можно объединять зоны 0.*

```
area 1 virtual-link 2.2.2.2
area 1 virtual-link 1.1.1.1
```

**Простая аутентификация на основе пароля позволяет обеспечить базовую защиту от несанкционированного доступа к OSPF-сообщениям.**

```
interface GigabitEthernet0/0
  ip ospf authentication-key mypassword
  ip ospf authentication
router ospf 1
  network 192.168.1.0 0.0.0.255 area 0
  area 0 authentication
```

**Пароли должны совпадать для интерфейсов. Также можно зашифровать ключ с помощью md5**

```
R1(config)#interface fastEthernet 0/1
R1(config-if)#ip ospf message-digest-key 1 md5 1234s
R1(config)#router ospf 1
R1(config-router)#area 0 authentication message-digest
```

------------
