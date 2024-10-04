# Настройка VTP

**Настройка сервера**:

```
Router> enable
Router# configure terminal
Router(conf)# vtp domain MyDomain
Router(conf)# vtp mode server
Router(conf)# vtp password MyPassword

##### Настройка trunk порта #####
Router(conf)# interface FastEthernet0/1
Router(conf-if)# switchport mode trunk
```

**Настройка клиента**

```
Router> enable
Router#configure terminal
Router(conf)# vtp domain MyDomain
Router(conf)# vtp mode client
Router(conf)#vtp password MyPassword

##### Настройка trunk порта #####
Router(conf)#interface FastEthernet0/1
Router(conf-if)#switchport mode trunk
```

---------
