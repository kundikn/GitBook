**Вход в привилегированный режим**

``` 
Switch> enable
```

**Переход в режим глобальной конфигурации**

``` 
Switch# configure terminal
```

**Настройка имени пользователя и пароля**

``` 
Switch(config)# username <имя_пользователя> privilege 15 secret <пароль>
```

**Зашифровать пароль**

``` 
Switch(config)# service password-encryption
```

**Настройте консольный доступ**

``` 
Switch(config)# line console 0
Switch(config-line)# login local
Switch(config-line)# exit
```

**Настройте telnet, управление по сети**

``` 
Switch(config)# line vty 0 4
Switch(config-line)# login local
Switch(config-line)# exit
```

**Настройка ssh**

``` 
Router(config)# ip domain-name example.com
Router(config)# crypto key generate rsa
Router(config)# line vty 0 4
Router(config-line)# transport input ssh
Router(config-line)# login local
```

**Убирает проверку введенно неправильных комманд**

``` 
Switch(config)# no ip domain-lookup
```

