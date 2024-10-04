# Настройка GRE

Создание GRE туннеля между двумя маршрутизаторами.

## На 1 коммутаторе

**Создаем виртуальный интерфейс и назначаем ip адрес**

```
Router(config)# interface tunnel 1
Router(config-if)# ip address 10.0.1.1 255.255.255.0
```

**Назначаем начало источника и конец источника**

```
Router(config-if)# tunnel source FastEthernet 0/1
Router(config-if)# tunnel destination 10.0.1.2
```

## На 2 коммутаторе

Создаем виртуальный интерфейс и назначаем ip адрес

```
Router(config)# interface tunnel 2
Router(config-if)# ip address 10.0.1.2 255.255.255.0
```

Назначаем начало источника и конец источника

```
Router(config-if)# tunnel source FastEthernet 0/1
Router(config-if)# tunnel destination 10.0.1.1
```

## Настройка маршрутизации

Потом нужно настроить маршруты через туннель

```
Router(config)# ip route 192.168.200.0 255.255.255.0 10.0.1.2
Router(config)# ip route 192.168.100.0 255.255.255.0 10.0.1.1
```

Можно настроить маршрутизацию через ospf

```
Router(config)# router ospf 1
Router(config)# router-id 1.1.1.1
Router(config)# network 192.168.10.0 0.0.0.255 area 0
Router(config)# network 10.0.1.0 0.0.0.255 area 0
```

-------
