# ACL (Access Control List)

**ACL (Access Control List)** на коммутаторах **Cisco** используются для фильтрации трафика на основе определенных критериев, таких как **IP-адреса**, **порты** и **протоколы**. **ACL** могут быть применены к интерфейсам коммутатора для контроля входящего и исходящего трафика.

## Типы ACL:

- **Standard ACL**: Фильтруют трафик на основе **IP-адреса** источника.
- **Extended ACL**: Фильтруют трафик на основе **IP-адреса** **источника**, **IP-адреса назначения**, **протокола**, **порта источника и порта назначения**.

------

## СОЗДАНИЕ СТАНДАРТНОГО ACL

**Создание стандартного access-list**

```R1(config)#ip access-list standard STVlan101
R1(config)#ip access-list standard STVlan101
```

**Запрещаем взаимодействие с сетью 192.168.102.0**

```R1(config)#ip access-list standard STVlan101
R1(config-std-nacl)#deny 192.168.102.0 0.0.0.255
```

**Запрещаем взаимодействие с хостом 192.168.200.10**

```R1(config)#ip access-list standard STVlan101
R1(config-std-nacl)#deny host 192.168.201.10
```

**Разрешить всё остальное**

```R1(config)#ip access-list standard STVlan101
R1(config-std-nacl)#permit any
```

**Привязываем ACL к подинтерфейсу на исходящий трафик**

```R1(config)#ip access-list standard STVlan101
R1(config-subif)#ip access-group STVlan101 out
```

----------

## СОЗДАНИЕ РАСШИРЕННОГО ACL

Создание расширенного списка доступов

```
R2(config)#ip access-list extended R2Servers1
```

Запрещаем icmp ото всех ко всем

```
R2(config-ext-nacl)#deny icmp any any
```

Разрешаем tcp ото всех ко всем

```
R2(config-ext-nacl)#permit tcp any any
```

Привязываем ACL к интерфейсу на исходящий трафик

```
R1(config-subif)#ip access-group STVlan101 out
```

-------

## СОЗДАНИЕ ИСКЛЮЧЕНИЙ ACL

Разрешаем icmp только для 192.168.101.101

```
R2(config-ext-nacl)#permit icmp host 192.168.101.101 any
```

Разрешаем http только для 192.168.200.20

```
R2(config-ext-nacl)#permit tcp any host 192.168.200.20 eq www
```

Запрещаем icmp ото всех ко всем

```
R2(config-ext-nacl)#deny icmp any any
```

Разрешаем tcp ото всех ко всем

```
R2(config-ext-nacl)#permit tcp any any
```

-------------
