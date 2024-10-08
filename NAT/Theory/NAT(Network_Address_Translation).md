# NAT (Network Address Translation)

**NAT** — это процесс, при котором один или несколько *частных* **IP-адресов** преобразуются в один или несколько *публичных* **IP-адресов**. Это обычно делается маршрутизатором или брандмауэром, который находится на границе сети. 

### Основные цели использования NAT включают

- **Экономия публичных IP-адресов**: Поскольку частные **IP-адреса** не маршрутизируются в Интернете, многие устройства в локальной сети могут использовать **один** публичный **IP-адрес** для выхода в Интернет.
- **Улучшение безопасности**: **NAT** *скрывает внутреннюю структуру сети от внешнего мира*, что может снизить риск несанкционированного доступа.
- **Управление трафиком**: **NAT** позволяет маршрутизаторам контролировать и фильтровать трафик между внутренней и внешней сетью.

---------

## PAT (Port Address Translation)

**PAT** — это расширение **NAT**, которое также известно как "*перегрузка NAT*" (**NAT overload**). **PAT** использует не только **IP-адреса**, *но и номера портов для различения сеансов связи*. Это позволяет *множеству устройств* в локальной сети использовать один *публичный* **IP-адрес**, но с разными портами для каждого соединения. 

-----------

### Основные особенности PAT включают

- **Многопользовательская поддержка**: **PAT** позволяет множеству устройств использовать один публичный **IP-адрес**, что значительно экономит публичные **IP-адреса**.
- **Динамическое назначение портов**: **PAT** *динамически назначает порты для каждого соединения*, что позволяет различать трафик от разных устройств.
- **Улучшение масштабируемости**: **PAT** улучшает масштабируемость сети, позволяя большому количеству устройств работать через ограниченное количество публичных **IP-адресов**.

---------
