# Настройка NAT

**Включение и настройка внутреннего NAT на интерфейсе fa0/0**

``` 
Router(config)#interface FastEthernet0/0
Router(config-if)#no shutdown
Router(config-if)#ip nat inside
Router(config-if)#end
```

**Включение и настройка внешнего NAT на интерфейсе fa1/0**

```
Router(config)#interface FastEthernet1/0
Router(config-if)#no shutdown
Router(config-if)#ip nat outside
Router(config-if)#end
```

**Настройка подмены адреса 192.168.10.1 на 95.99.99.13 при прохождении через маршрутизатор (Статический вариант)**

```
Router(config)#ip nat inside source static 192.168.10.1 95.99.99.13
```

**Создаем список доступа для определения, какие IP-адреса из внутренней сети будут преобразовываться с использованием NAT (Динамический вариант)**

```
Router(config)# access-list 1 permit 192.168.10.0 0.0.0.255
```

**Показать списки**

```
Router#show access-lists
```

**Удалить список**

```
Router(config)# ip access-list standard 2
Router(config-std-nacl)# no 10
```

**Добавить в список подсеть**

```
Router(config-std-nacl)# permit 192.168.40.0 0.0.0.255
```

**Определите пул внешних IP-адресов, которые роутер будет использовать для преобразования**

```
Router(config)# ip nat pool MY_POOL 203.0.113.2 203.0.113.10 netmask 255.255.255.0
```

**Теперь нужно связать наш ACL с пулом NAT**

```
Router(config)# ip nat inside source list 1 pool MY_POOL overload
```

**Создание nat с помощью overload, где ты указываешь не пул внешних адресов, а интерфейс через который будет ходить (то есть если ip адрес на интерфейсе будет динамическим)**

```
Router(config)#ip access-list standard mynat
Router(config-std-nacl)#permit 192.168.10.0 0.0.0.255
Router(config-std-nacl)#exit
Router(config)#ip nat inside source list mynat interface fa1/0 overload
```

--------
