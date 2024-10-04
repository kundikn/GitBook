# Настройка IPsec

**Активируем политику безопасности, которая позвляет использовать доп возможности шифрования и аутентификации данных isakmp для настройки IPsec**

```
Router(config)# license boot module c1900 technology-package securityk9
Router# write
Router# reload
```

**Установка протокола IKE, который используется для того, чтобы установить защищенный канал связи между двумя сетями**

```
Router(config)# crypto isakmp policy 10
```

**Параметры политики**

```
Router(config-isakmp)# encryption aes
Router(config-isakmp)# hash sha
Router(config-isakmp)# authentication pre-share
Router(config-isakmp)# group 2
Router(config-isakmp)# lifetime 86400
```

**Настройка предустановленного ключа, адрес - интерфейс маршрутизатора на который будут приходить пакеты**

```
Router(config)# crypto isakmp key cisco123 address 192.168.1.2
```

**Создание набора трансформаций (правил) IPsec**

```
Router(config)# crypto ipsec transform-set MY_TRANSFORM_SET esp-aes esp-sha-hmac
```

**Настройка расширенное ACL для определения трафика, который будет защищен**

```
Router(config)# access-list 101 permit ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
```

**Создание карты (профиля) IPsec**

```
Router(config)# crypto map MY_CRYPTO_MAP 10 ipsec-isakmp
```

**Установка параметров профиля IPsec**

```
Router(config-crypto-map)# set peer 192.168.1.2
Router(config-crypto-map)# set transform-set MY_TRANSFORM_SET
Router(config-crypto-map)# match address 101
```

**Привязываем карту к интерфейсу**

```
Router(config-if)# crypto map MY_CRYPTO_MAP
```

----------
