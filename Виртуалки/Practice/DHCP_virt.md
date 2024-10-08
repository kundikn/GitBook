# Настройка DHCP на виртуалке

Поставить сервер и настроить его как **DHCP**.
Если есть несколько **VLAN**, то **DHCP** сервер не будет найден, так как **DHCP** ведётся на *канальном уровне*, а одно из свойств маршрутизатора - ограничение трафика канального уровня в пределах одной локальной сети, то есть у нас **DHCP **в другом **VLAN**, следовательно до него не достучаться.
Поможет решить настройка на маршрутизаторе, она отправляет все широковещательные запросы на указанный адресс (**DHCP relay**):

```R1(config-subif)#ip helper-address 192.168.103.10 
R1(config-subif)#ip helper-address 192.168.103.10 
```
-------
## Настройка конфига isc-dhcp-server

При установке **isc-dhcp-server** он будет сразу выдавать ошибку, так как сервер еще не настроен

Нужно в файле */etc/default/isc-dhcp-linux* указать активный интерфейс, иначе не запустится **dhcp**

```
INTERFACES="eth0"
```

## Настройка dhcp на линуксе для нескольких влан

Для **Ubuntu** и **Debian** установка пакета **isc-dhcp-server** (для других дистрибутивов другое). 

**Пример конфигурации dhcp находится в файле /etc/dhcp/dhcpd.conf**:

```
# dhcpd.conf
#

# Sample configuration file for Is dhopd
#

# option definitions common to all supported networks...
option domain-name-servers 8.8.8.8, 1.1.1.45

min-lease-time 36005
default-lease-time 32400;
max-lease-time 6048005

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;

# No service will he given on this subnet, but declaring it helps the
# DHCP server to understand the network topology.

#subnet 10.152.187.0 netmask 255.255.255.0 {

subnet 192.168.10.0 netmask 255.255.255.0 {
range 192.168.10.100 192.168.10.200;
option routers 192.168.10.1

subnet 192.168.20.0 netmask 255.255.255.0 {
range 192.168.20.100 192.168.20.200;
option routers 192. 168.20.13

```

## DHCP Snooping

Одна из защит от атак по **DHCP**.

**Включить DHCP Snooping**

```
$ configure terminal
$ ip dhcp snooping
```

**Настроить на vlan**

```
$ ip dhcp snooping vlan 10,20
```

**На этом порту находится dhcp сервер, указываем его доверенным**

```
$ interface GigabitEthernet0/1
$ ip dhcp snooping trust
```

-------------

