# Troubleshoot DPR

**Проверка соседства (adjacency)**

```
Router# show ip eigrp neighbors
```

**Проверка таблицы маршрутизации**

```
Router# sh ip route eigrp
```

**Проверка состояния интерфейсов**

```
Router# show ip interface brief
```

**Проверка анонсируемых маршрутов, тут у EIGRP можно посмотреть FD**

```
Router# show ip eigrp topology
```

**Проверка согласованности масок и метрик. Убедится, что у всех маршрутизаторов внутри одной области или сети используются одинаковые маски и настройки.**

**Проверка доступа (ACL, фильтры), что ничего не блокируют.**

**Использование debug для отслеживания проблемы**

```
Router# debug eigrp packets
Router# undebug all
```

**Синхронизация времени (NTP)**

----------
