# Настройка VLAN 

**Посмотреть информацию о VLAN**

```
SW1#show vlan brief
```

**Создаем VLAN**

```
SW1(config)#vlan 101
SW1(config-vlan)#name Depart1
```

**Указываем в каком режиме будет работать порт**

```
SW1(config)#interface fastEthernet 0/1
SW1(config-if)#switchport mode access 
```

**Указываем vlan**

```
SW1(config-if)#switchport access vlan 101
```

**Порт в режиме работы транк, то есть принимает все VLAN**

```
SW1(config-if)#switchport mode trunk
```

**Указывает VLAN**

```
SW1(config-if)#switchport trunk native vlan 100
```

**На маршрутизаторе создаем под интерфейс**

```
R1(config-if)#int fa 0/0.101
```

**Нужно обьснить что интерфейс 0/0.101 находится в vlan 101**

```
R1(config-subif)#encapsulation dot1Q 101
```

**Указываем ip адресс этому интерфейсу**

```
R1(config-subif)#ip address 192.168.100.1 255.255.255.0
```

--------
