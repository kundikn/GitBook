# Настройка протокола BGP

Сейчас будут предоставлены конфиги 4 роутеров на которых запщуен протокол динамической маршрутизации BGP.

**R1:**

```
router bgp 64500
 	no synchronization
 	bgp log-neighbor-changes
 	network 192.168.101.0
 	neighbor 10.10.10.2 remote-as 64501
 	neighbor 10.10.13.1 remote-as 64503
 	neighbor 10.10.20.2 remote-as 64502
 	no auto-summary
```

**R2:**

```
router bgp 64501
 	no synchronization
 	bgp log-neighbor-changes
 	network 192.168.102.0
 	neighbor 10.10.10.1 remote-as 64500
 	neighbor 10.10.11.2 remote-as 64502
 	no auto-summary
```

**R3:**

```
router bgp 64502
 	no synchronization
 	bgp log-neighbor-changes
 	network 192.168.103.0
 	neighbor 10.10.11.1 remote-as 64501
 	neighbor 10.10.12.2 remote-as 64503
 	neighbor 10.10.20.1 remote-as 64500
 	no auto-summary
```

**R4:**

```
router bgp 64503
 	no synchronization
 	bgp log-neighbor-changes
 	network 192.168.104.0
 	neighbor 10.10.12.1 remote-as 64502
 	neighbor 10.10.13.2 remote-as 64500
 	no auto-summary
```

*Надо указать, что есть у нас вот эта сеточка и передать её провайдерам?*
Для этого существует три варианта:

- определить сети командой **network** (*больший приоритет*)
- импортировать из **другого источника** *(direct, static, IGP*)
- создать агрегированный маршрут командой **aggregate-address**

--------
