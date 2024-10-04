# Настройка STP на коммутаторах

*PVST в cisco включен по умолчанию*

**Сделать коммутатор рутовым для первого vlan**

```
Switch(config)#spanning-tree vlan 1 root primary
```

**Назначение самого высокого приоритета**

```
Switch(config)#spanning-tree vlan 1 priority 0
```

**Включить протокол rapid pvst для быстрой сходимости при изменении типологии сети**

```
Switch(config)#spanning-tree mode rapid-pvst
```

## Фильтрация MAC адресов

**Заходим на порт**

```
Switch(config)#int fastEthernet 0/1
```

**Включение функциональности безопасности порта на коммутаторе**

```
Switch(config-if)# switchport port-security
```

**Максимальное значение мак адресов**

```
Switch(config-if)# switchport port-security maximum 1
```

**Запоминает последний mac который обращался (можно также прописать mac вручную)**

```
Switch(config-if)#switchport port-security mac-address sticky 
```

**Посмотреть настройки безопасности порта**

```
Switch#sh port-security int fastEthernet 0/1
```

**Посмотреть mac адреса на портах**

```
Switch#show mac address-table
```

***Violation mode** - настройка у порт security, отвечающая на то, как порт должен реагировать при несанкционированный доступ на порт*

**Запрещает передачу пакетов от данного mac адреса, но порт не выключает, и создает snmp сообщение администратору сети**

```
Switch(config-if)#switchport port-security violation restrict 
```

**Запрещает передачу пакетов от данного mac адреса, но порт не выключает, не создает snmp сообщения**

```
Switch(config-if)#switchport port-security violation protect
```

--------
