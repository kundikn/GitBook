# Сети для самых маленьких ![GitHub stars](https://img.shields.io/github/stars/kundikn/Gitbook) ![License](https://img.shields.io/github/license/kundikn/Gitbook) ![GitHub forks](https://img.shields.io/github/forks/kundikn/Gitbook)

*Это руководство хороший старт в сетях*

## Оглавление
- AAA<br>
- ACL<br>
- NAT
  - Теория<br>
  - Практика<br>
- Траблшутинг протоколов<br>
- VPN
  - Теория<br>
  - Практика<br>
- Виртуалки под разные задачи<br>
  - Теория<br>
  - Практика<br>
- Интересная теория<br>
- Протоколы коммутаторов<br>
  - Теория<br>
  - Практика<br>
- Протоколы маршрутизации<br>
  - Теория<br>
    - BGP<br>
  - Практика<br>
---------

## Содержание

- AAA (Authentication, Authorization, Accounting)
   - [Как настроить удаленное подключение к коммутатору](./AAA/auth_ssh.md)
- ACL (Access Control List)
   - [Создание ACL листов](./ACL/ACL(Access_Control_List).md)
- NAT
  - Теория
    - [NAT](./NAT/Theory/NAT(Network_Address_Translation).md)
  - Практика
    - [Настройка NAT](./NAT/Practice/Настройка_NAT.md)
- Траблшутинг протоколов
  - [Трабшутинг протокола BGP](./Troubleshooting/Troubleshoot_BGP.md)
  - [Трабшутинг протоколов IGP](./Troubleshooting/Troubleshoot_IGP.md)
  - [Трабшутинг протокола BGP](./Troubleshooting/Troubleshooting_DMVPN.md)
- VPN
  - Теория
    - [DMVPN](./VPN/Theory/DMVPN(Dynamic_Multipoint_VPN).md)
    - [GRE](./VPN/Theory/GRE(Generic_Routing_Encapsulation).md)
    - [GRE over IPsec](./VPN/Theory/GRE_over_IPSec.md)
    - [IPsec](./VPN/Theory/IPsec(Internet_Protocol_Security).md)
    - [IPsec VTI](./VPN/Theory/IPSecVTI(Virtual_Tunnel_Interface).md)
  - Практика
     - [Настройка DMVPN](./VPN/Practice/Настройка_DMVPN.md)
     - [Настройка GRE](./VPN/Practice/Настройка_GRE.md)
     - [Настройка IPsec](./VPN/Practice/Настройка_IPsec.md)
- Виртуалки под разные задачи
  - Теория
    - [Что такое DHCP?](./Виртуалки/Theory/DHCP(Dynamic_Host_Configuration_Protocol).md)
  - Практика
    - [Настройка DHCP на Linux](./Виртуалки/Practice/DHCP_virt.md)
    - [Настройка DHCP на маршрутизаторе](./Виртуалки/Practice/DHCP_route.md)
    - [Настройка SYSLOG на Linux](./Виртуалки/Practice/SYSLOG.md)
- Интересная теория
  - [DMZ](./Theory/DMZ(DeMilitarized_Zone).md)
  - [Агрегирование портов](./Theory/Агрегирование_портов.md)
  - [Классы сетей.md](./Theory/Классы_сетей.md)
- Протоколы коммутаторов
  - Теория
    - [Протокол STP](./Switch_protocols/Theory/STP(Spanning_Tree_Protocol).md)
    - [Протокол VLAN](./Switch_protocols/Theory/VLAN(Virtual_Local_Area_Network).md)
    - [Протокол VTP](./Switch_protocols/Theory/VTP(VLAN_Trunking_Protocol).md)
  - Практика
    - [Настройка STP на коммутаторах](./Switch_protocols/Practice/Настройка_STP_на_коммутаторах.md)
    - [Настройка VLAN на коммутаторах](./Switch_protocols/Practice/Настройка_VLAN.md)
    - [Настройка VTP на коммутаторах](./Switch_protocols/Practice/Настройка_VTP.md)
 - Протоколы маршрутизации
   - Теория
     - [Что такое маршрутизация?](./Router_protocols/Theory/Маршрутизация.md)
     - [Прокотол динамичечской маршрутизации RIP](./Router_protocols/Theory/RIP(Routing_Information_Protocol).md)
     - [Прокотол динамичечской маршрутизации OSPF](./Router_protocols/Theory/OSPF(Open_Shortest_Path_First).md)
     - [Прокотол динамичечской маршрутизации EIGRP](./Router_protocols/Theory/EIGRP(Enhanced_Interior_Gateway_Routing_Protocol).md)
     - [Балансировка нагрузки в OSPF и EIGRP](./Router_protocols/Theory/Балансировка_нагрузки_OSPF_EIGRP.md)
     - [PBR (не совсем протокол, но используется в перераспределении нагрузки)](./Router_protocols/Theory/PBR(Policy-Based_Routing).md)
     - BGP
       - [Протокол BGP](./Router_protocols/Theory/BGP/BGP(Border_Gateway_Protocol).md)
       - [Выбор маршрута в BGP](./Router_protocols/Theory/BGP/BGP_выбор_маршрута.md)
       - [Балансировка и распределение нагрузки в BGP](./Router_protocols/Theory/BGP/Балансировка_и_распределение_нагрузки_BGP.md)
       - [Управление маршрутами в BGP](./Router_protocols/Theory/BGP/Управление_маршрутами_BGP.md)
   - Практика
     - [Настройка статической маршрутизации](/Router_protocols/Practice/Настройка_статической_маршрутизации.md)
     - [Настройка протокола RIP](/Router_protocols/Practice/Настройка_RIP.md)
     - [Настройка протокола OSPF](/Router_protocols/Practice/Настройка_OSPF.md)
     - [Настройка протокола BGP](/Router_protocols/Practice/Настройка_протокола_BGP.md)
       
 ## Как внести вклад
*Мы приветствуем ваш вклад! Пожалуйста, отправляйте pull requests или создавайте issues для обсуждения предложений.*    
