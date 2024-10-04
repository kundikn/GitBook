# Настройка DHCP на маршрутизаторе

**Исключаем адреса из пула адресов DHCP**

```
R1(config)#ip dhcp excluded-address 192.168.101.254
R1(config)#ip dhcp excluded-address 192.168.102.254
R1(config)#ip dhcp excluded-address 192.168.101.1 192.168.101.10
R1(config)#ip dhcp excluded-address 192.168.102.1 192.168.102.10
```

**Включаем непосредственно DHCP**

```
R1(config)#ip dhcp pool R1Vlan101
```

**Номер сети в котором будет находиться пулл**

```
R1(dhcp-config)#network 192.168.101.0 255.255.255.0
```

**Шлюз по умолчанию**

```
R1(dhcp-config)#default-router 192.168.101.254
```

**Указание DNS сервера**

````
R1(dhcp-config)#dns-server 192.168.101.250
````

---------
