### ДЗ9. Конфигурация безопасности коммутатора.
--------------------

![](Топология_9.PNG)

--------------
### Задание.

-------
 [Часть 1. Настройка основного сетевого устройства.](README.md#часть-1-настройка-основного-сетевого-устройства)

* Создайте сеть.
* Настройте маршрутизатор R1.
* Настройка и проверка основных параметров коммутатора.

[Часть 2. Настройка сетей VLAN.](README.md#2-настройка-сетей-vlan-на-коммутаторах)
* Сконфигруриуйте VLAN 10.
* Сконфигруриуйте SVI для VLAN 10.
* Настройте VLAN 333 с именем Native на S1 и S2.
* Настройте VLAN 999 с именем ParkingLot на S1 и S2.

[Часть 3: Настройки безопасности коммутатора.](README.md#3-настройки-безопасности-коммутатора)
* Реализация магистральных соединений 802.1Q.
* Настройка портов доступа
* Безопасность неиспользуемых портов коммутатора
* Документирование и реализация функций безопасности порта.
* Реализовать безопасность DHCP snooping .
* Реализация PortFast и BPDU Guard
* Проверка сквозной связанности.

-----------------------
### Решение.
-----------------------

### Часть 1. Настройка основного сетевого устройства/

#### Шаг 1. Создайте сеть.
a.	Создайте сеть согласно топологии.

![](Топология_9_1.PNG)

b.	Инициализация устройств

-------
#### Шаг 2. Настройте маршрутизатор R1.

--------------
Проведем базовую настройку маршрутизатора R1


```
Router(config)#hostname R1
R1(config)#no ip domain lookup
R1(config)#ip dhcp excluded-address 192.168.10.1 192.168.10.9
R1(config)#ip dhcp excluded-address 192.168.10.201 192.168.10.202
R1(config)#
R1(config)#
R1(config)#ip dhcp pool Students
R1(dhcp-config)# network 192.168.10.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.10.1
R1(dhcp-config)# domain-name CCNA2.Lab-11.6.1
R1(dhcp-config)#exit
R1(config)#
R1(config)#
R1(config)#interface Loopback0

R1(config-if)# ip address 10.10.1.1 255.255.255.0
R1(config-if)#
%LINK-5-CHANGED: Interface Loopback0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback0, changed state to up

R1(config-if)#exit
R1(config)#int gig0/0/1
R1(config-if)#
R1(config-if)#description Link to S1
R1(config-if)#
R1(config-if)#ip address 192.168.10.1 255.255.255.0
R1(config-if)#no sh
R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#exit
R1(config)#
R1(config)#line con 0
R1(config-line)#
R1(config-line)#logging synchronous 
R1(config-line)#
R1(config-line)#exec-timeout 5 0
R1(config-line)#
R1(config-line)#exit
R1(config)#exit
R1#

```

Проверьте текущую конфигурацию на R1.

```
R1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   192.168.10.1    YES manual up                    up 
Loopback0              10.10.1.1       YES manual up                    up 
Vlan1                  unassigned      YES NVRAM  administratively down down
R1#
```
IP-адресация и интерфейсы находятся в состоянии up / up.

Проверим работу DHCP сервера

```
R1(config)#do show ip dhcp pool

Pool Students :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 254
 Leased addresses               : 2
 Excluded addresses             : 2
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.10.1         192.168.10.1     - 192.168.10.254    2    / 2     / 254
R1(config)#
R1#show ip dhcp bin
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.10.10    00D0.BCA4.121B           --                     Automatic
192.168.10.11    0007.EC07.8CA9           --                     Automatic
R1#
```
ПК PC-A и PC-B получили адреса DHCP.

-----------------------
#### Шаг 3. Настройка и проверка основных параметров коммутатора



a.	Настройте имя хоста для коммутаторов S1 и S2.  

b.	Запретите нежелательный поиск в DNS.
c.	Настройте описания интерфейса для портов, которые используются в S1 и S2.

d.	Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.

-----------
### 2. Настройка сетей VLAN на коммутаторах.

------------

Шаг 1. Сконфигруриуйте VLAN 10.


Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.

Шаг 2. Сконфигруриуйте SVI для VLAN 10.  

Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 
и S2. Включите интерфейсы SVI и предоставьте описание для интерфейса.

Шаг 3. Настройте VLAN 333 с именем Native на S1 и S2.

Шаг 4. Настройте VLAN 999 с именем ParkingLot на S1 и S2.

```
SW1#
SW1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1(config)#
SW1(config)#vlan 10
SW1(config-vlan)#
SW1(config-vlan)#name Management
SW1(config-vlan)#
SW1(config-vlan)#vlan 333
SW1(config-vlan)#
SW1(config-vlan)#name Native
SW1(config-vlan)#
SW1(config-vlan)#vlan 999
SW1(config-vlan)#
SW1(config-vlan)#name ParkingLot 
SW1(config-vlan)#
SW1(config-vlan)#exit
SW1(config)#
SW1(config)#
SW1(config)#int vlan 10
SW1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

SW1(config-if)#
SW1(config-if)#ip address 192.168.10.201 255.255.255.0
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#exit
SW1#

SW1#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
10   Management                       active    
333  Native                           active    
999  ParkingLot                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active  

SW1#
```
Аналогичные настройки проведем на коммутаторе SW2.

--------------
### 3. Настройки безопасности коммутатора.

--------------

#### Шаг 1. Релизация магистральных соединений 802.1Q.

a.	Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.
```
SW1(config)#
SW1(config)#int fa0/1
SW1(config-if)#
SW1(config-if)#switch
SW1(config-if)#switchport mode trunk

SW1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up

SW1(config-if)#
SW1(config-if)#switchport trunk native vlan 333
SW1(config-if)#
SW1(config-if)#
```


b.	Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.

Коммутатор SW1
```
SW1#
SW1#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,333,999
```

Коммутатор SW2

```
SW2#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,333,999

SW2#
```

c. Указываем, что VLAN 10  может проходить по транку, запрещаем прохождение VLAN 1 и отключаем Протокол динамического транкинга (DTP).
```
SW1(config)#
SW1(config)#int fa0/1
SW1(config-if)#
SW1(config-if)#switchport trunk allowed vlan 10
SW1(config-if)#
SW1(config-if)#switchport nonegotiate
SW1(config-if)#exit
SW1(config)#exit
SW1#
```

```

SW1#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       10

Port        Vlans allowed and active in management domain
Fa0/1       10

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       10

SW1#
SW1#show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
SW1#

```
-----

#### Шаг 2. Настройка портов доступа и неиспользуемых портов.

--------------
a.	На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.

b.	На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.

c.	На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите неиспользуемые порты.

d.	Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999



Коммутатор SW1

```
SW1(config)#int range fa0/2-24,gig0/1-2
SW1(config-if-range)#
SW1(config-if-range)#switchport mode access 
SW1(config-if-range)#
SW1(config-if-range)#exit
SW1(config)#int range fa0/2-4,fa0/7-24,gig0/1-2
SW1(config-if-range)#
SW1(config-if-range)#switchport access vlan 999
SW1(config-if-range)#
SW1(config-if-range)#shut

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/18, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
SW1(config-if-range)#exit
SW1(config)#int range fa0/5-6
SW1(config-if-range)#
SW1(config-if-range)#switchport access vlan 10
SW1(config-if-range)#
SW1(config-if-range)#exit
SW1(config)#exit
SW1#
%SYS-5-CONFIG_I: Configured from console by console

SW1#
SW1#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
10   Management                       active    Fa0/5, Fa0/6
333  Native                           active    
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

SW1#sh int status 
Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    trunk      auto    auto  10/100BaseTX
Fa0/2                        disabled 999        auto    auto  10/100BaseTX
Fa0/3                        disabled 999        auto    auto  10/100BaseTX
Fa0/4                        disabled 999        auto    auto  10/100BaseTX
Fa0/5                        connected    10         auto    auto  10/100BaseTX
Fa0/6                        connected    10         auto    auto  10/100BaseTX
Fa0/7                        disabled 999        auto    auto  10/100BaseTX
Fa0/8                        disabled 999        auto    auto  10/100BaseTX
Fa0/9                        disabled 999        auto    auto  10/100BaseTX
Fa0/10                       disabled 999        auto    auto  10/100BaseTX
Fa0/11                       disabled 999        auto    auto  10/100BaseTX
Fa0/12                       disabled 999        auto    auto  10/100BaseTX
Fa0/13                       disabled 999        auto    auto  10/100BaseTX
Fa0/14                       disabled 999        auto    auto  10/100BaseTX
Fa0/15                       disabled 999        auto    auto  10/100BaseTX
Fa0/16                       disabled 999        auto    auto  10/100BaseTX
Fa0/17                       disabled 999        auto    auto  10/100BaseTX
Fa0/18                       disabled 999        auto    auto  10/100BaseTX
Fa0/19                       disabled 999        auto    auto  10/100BaseTX
Fa0/20                       disabled 999        auto    auto  10/100BaseTX
Fa0/21                       disabled 999        auto    auto  10/100BaseTX
Fa0/22                       disabled 999        auto    auto  10/100BaseTX
Fa0/23                       disabled 999        auto    auto  10/100BaseTX
Fa0/24                       disabled 999        auto    auto  10/100BaseTX
Gig0/1                       disabled 999        auto    auto  10/100BaseTX
Gig0/2                       disabled 999        auto    auto  10/100BaseTX

SW1#
```
Коммутатор SW2

```
SW2(config)#
SW2(config)#int range fa0/2-24,gig0/1-2
SW2(config-if-range)#
SW2(config-if-range)#switchport mode access 
SW2(config-if-range)#
SW2(config-if-range)#exit
SW2(config)#int range fa0/2-17,fa0/19-24,gig0/1-2
SW2(config-if-range)#
SW2(config-if-range)#switchport access vlan 999
SW2(config-if-range)#
SW2(config-if-range)#shut

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/5, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/6, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/7, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/8, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/9, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/10, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/11, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/12, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/13, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/14, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/15, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/16, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/17, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/19, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/20, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/21, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/22, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/23, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/24, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
SW2(config-if-range)#
SW2(config-if-range)#exit
SW2(config)#int fa0/18
SW2(config-if)#switchport access vlan 10
SW2(config-if)#
SW2(config-if)#exit
SW2(config)#
SW2(config)#exit
SW2#
%SYS-5-CONFIG_I: Configured from console by console

SW2#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
10   Management                       active    Fa0/18
333  Native                           active    
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    


SW2#
SW2#sh int status 
Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    trunk      auto    auto  10/100BaseTX
Fa0/2                        disabled 999        auto    auto  10/100BaseTX
Fa0/3                        disabled 999        auto    auto  10/100BaseTX
Fa0/4                        disabled 999        auto    auto  10/100BaseTX
Fa0/5                        disabled 999        auto    auto  10/100BaseTX
Fa0/6                        disabled 999        auto    auto  10/100BaseTX
Fa0/7                        disabled 999        auto    auto  10/100BaseTX
Fa0/8                        disabled 999        auto    auto  10/100BaseTX
Fa0/9                        disabled 999        auto    auto  10/100BaseTX
Fa0/10                       disabled 999        auto    auto  10/100BaseTX
Fa0/11                       disabled 999        auto    auto  10/100BaseTX
Fa0/12                       disabled 999        auto    auto  10/100BaseTX
Fa0/13                       disabled 999        auto    auto  10/100BaseTX
Fa0/14                       disabled 999        auto    auto  10/100BaseTX
Fa0/15                       disabled 999        auto    auto  10/100BaseTX
Fa0/16                       disabled 999        auto    auto  10/100BaseTX
Fa0/17                       disabled 999        auto    auto  10/100BaseTX
Fa0/18                       connected    10         auto    auto  10/100BaseTX
Fa0/19                       disabled 999        auto    auto  10/100BaseTX
Fa0/20                       disabled 999        auto    auto  10/100BaseTX
Fa0/21                       disabled 999        auto    auto  10/100BaseTX
Fa0/22                       disabled 999        auto    auto  10/100BaseTX
Fa0/23                       disabled 999        auto    auto  10/100BaseTX
Fa0/24                       disabled 999        auto    auto  10/100BaseTX
Gig0/1                       disabled 999        auto    auto  10/100BaseTX
Gig0/2                       disabled 999        auto    auto  10/100BaseTX

SW2#
```
---------
#### Шаг 3. Документирование и реализация функций безопасности порта.

----------
>Интерфейсы F0/6 на S1 и F0/18 на S2 настроены как порты доступа. На этом шаге вы также настроите безопасность портов на этих двух портах доступа.


a.	На S1, введите команду **show port-security interface f0/6**  для отображения настроек по умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.

```
SW1#show port-security interface fa0/6
Port Security              : Disabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
SW1#
```
Конфигурация безопасности порта по умолчанию.

|Функция|Настройка|
|:----|:--------:|
|Защита портов|  Disabled   |   
|Макс. кол-во записей MAC адресов| 1 |
| Режим проверки на нарушение безопасности| Shutdown |
| Aging Time|  0 mins |
|Aging Type| Absolute |
|Secure Static Address Aging| Disabled |
|Sticky MAC Address| 0 |

b.	На S1 включите защиту порта на F0 / 6 со следующими настройками:
* Максимальное количество записей MAC-адресов: 3
* Режим безопасности: restrict
* Aging time: 60 мин.
* Aging type: неактивный - настройка недоступна в CPT данной версии.

```
SW1(config)#
SW1(config)#
SW1(config)#int fa0/6
SW1(config-if)#
SW1(config-if)#switchport port-security
SW1(config-if)#
SW1(config-if)#switchport  port-security maximum 3
SW1(config-if)#
SW1(config-if)#switchport  port-security ?
  aging        Port-security aging commands
  mac-address  Secure mac address
  maximum      Max secure addresses
  violation    Security violation mode
  <cr>
SW1(config-if)#switchport  port-security violation restrict 
SW1(config-if)#
SW1(config-if)#switchport  port-security aging time 60
SW1(config-if)#



SW1#
SW1#show port-security interface f0/6
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 60 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0

SW1#

SW1#show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)
----    -----------       ----                          -----   -------------
10	00D0.BCA4.121B	DynamicConfigured	FastEthernet0/6		-
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 1024
SW1#
```
c.	Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую конфигурацию.


e.	Настройте следующие параметры безопасности порта на S2 F / 18:

* Максимальное количество записей MAC-адресов: 2
* Тип безопасности: Protect
* Aging time: 60 мин.

```
SW2(config)#
SW2(config)#int fa0/18
SW2(config-if)#
SW2(config-if)#switchport port-security
SW2(config-if)#
SW2(config-if)#switchport  port-security maximum 2
SW2(config-if)#
SW2(config-if)#switchport  port-security violation protect 
SW2(config-if)#
SW2(config-if)#switchport  port-security aging time 60
SW2(config-if)#
SW2(config-if)#switchport port-security mac-address sticky
SW2(config-if)#
SW2(config-if)#do show port-security interface f0/18

Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Protect
Aging Time                 : 60 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 1
Last Source Address:Vlan   : 0007.EC07.8CA9:10
Security Violation Count   : 0

SW2#
SW2#show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)
----    -----------       ----                          -----   -------------
  10    0007.EC07.8CA9    SecureSticky                  Fa0/18       -
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 1024
SW2#
```
--------

#### Шаг 5. Реализовать безопасность DHCP snooping.

------------------
a.	На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10.

```
SW2(config)#
SW2(config)#
SW2(config)#ip dhcp snooping
SW2(config)#
SW2(config)#ip dhcp snooping vlan 10
SW2(config)#
SW2(config)#no ip dhcp snooping information option 
SW2(config)#
```

b.	Настройте магистральные порты на S2 как доверенные порты.

```
SW2(config)#
SW2(config)#int fa0/1
SW2(config-if)#
SW2(config-if)#ip dhcp snooping trust
SW2(config-if)#
SW2(config-if)#exit
SW2(config)#exit
SW2#
%SYS-5-CONFIG_I: Configured from console by console

SW2#
SW2#sh ip dhcp snooping 
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10
Insertion of option 82 is disabled
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Interface                  Trusted    Rate limit (pps)
-----------------------    -------    ----------------
FastEthernet0/1            yes        unlimited       
FastEthernet0/18           no         unlimited       
SW2#
```

c.	Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.

```
SW2(config)#int fa0/18
SW2(config-if)#
SW2(config-if)#
SW2(config-if)#ip dhcp snooping limit rate 5
SW2(config-if)#
```


d.	Проверка DHCP Snooping на S2.
```
SW2#
SW2#sh ip dhcp snooping 
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10
Insertion of option 82 is disabled
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Interface                  Trusted    Rate limit (pps)
-----------------------    -------    ----------------
FastEthernet0/1            yes        unlimited       
FastEthernet0/18           no         5               


SW2#
SW2#sh ip dhcp snooping bin
SW2#sh ip dhcp snooping binding 
MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  -----------------
00:07:EC:07:8C:A9   192.168.10.11    0           dhcp-snooping  10    FastEthernet0/18
Total number of bindings: 1
SW2#
```
e.	В командной строке на PC-B освободите, а затем обновите IP-адрес.
```
C:\>ipconfig /release

   IP Address......................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: 0.0.0.0
   DNS Server......................: 0.0.0.0

C:\>ipconfig /renew

   IP Address......................: 192.168.10.11
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: 192.168.10.1
   DNS Server......................: 0.0.0.0

C:\>
```
--------------
#### Шаг 6. Реализация PortFast и BPDU Guard
--------------

a.	Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах и включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.

```
SW2#
SW2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW2(config)#
SW2(config)#int fa0/18
SW2(config-if)#
SW2(config-if)#spanning-tree portfast 
%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc... to this
interface  when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION

%Portfast has been configured on FastEthernet0/18 but will only
have effect when the interface is in a non-trunking mode.
SW2(config-if)#spanning-tree bpduguard enable 
SW2(config-if)#
SW2(config-if)#
```
```
SW1(config)#
SW1(config)#int fa0/6
SW1(config-if)#
SW1(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc... to this
interface  when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION

%Portfast has been configured on FastEthernet0/6 but will only
have effect when the interface is in a non-trunking mode.
SW1(config-if)#
SW1(config-if)#spanning-tree bpduguard enable
SW1(config-if)#^Z
SW1#

```
b.	Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.
```
SW1#show spanning-tree int fa0/6 detail


Port 6 (FastEthernet0/6) of VLAN0010 is designated forwarding
  Port path cost 19, Port priority 128, Port Identifier 128.6
  Designated root has priority 32778, address 0001.6329.2A53
  Designated bridge has priority 32778, address 0001.6329.2A53
  Designated port id is 128.6, designated path cost 19
  Timers: message age 16, forward delay 0, hold 0
  Number of transitions to forwarding state: 1
  The port is in the portfast mode
  Link type is point-to-point by default

SW1#
```

```
SW2#show spanning-tree int fa0/18 detail


Port 18 (FastEthernet0/18) of VLAN0010 is designated forwarding
  Port path cost 19, Port priority 128, Port Identifier 128.18
  Designated root has priority 32778, address 0001.6329.2A53
  Designated bridge has priority 32778, address 000D.BD25.E02D
  Designated port id is 128.18, designated path cost 19
  Timers: message age 16, forward delay 0, hold 0
  Number of transitions to forwarding state: 1
  The port is in the portfast mode
  Link type is point-to-point by default

SW2#
```
-------
#### Шаг 7. Проверьте наличие сквозного ⁪подключения.

-----------
Проверьте PING свзяь между всеми устройствами в таблице IP-адресации. 

PING с ПК PC-A

```
C:\>
C:\>ping 192.168.10.1

Pinging 192.168.10.1 with 32 bytes of data:

Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.10.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.10.201

Pinging 192.168.10.201 with 32 bytes of data:

Request timed out.
Reply from 192.168.10.201: bytes=32 time<1ms TTL=255
Reply from 192.168.10.201: bytes=32 time<1ms TTL=255
Reply from 192.168.10.201: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.10.201:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.10.202

Pinging 192.168.10.202 with 32 bytes of data:

Request timed out.
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.10.202:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.10.11

Pinging 192.168.10.11 with 32 bytes of data:

Reply from 192.168.10.11: bytes=32 time=15ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128
Reply from 192.168.10.11: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.10.11:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 15ms, Average = 3ms
```

PING  с ПК PC-B

```
C:\>ping 192.168.10.1

Pinging 192.168.10.1 with 32 bytes of data:

Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.10.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.10.201

Pinging 192.168.10.201 with 32 bytes of data:

Request timed out.
Reply from 192.168.10.201: bytes=32 time<1ms TTL=255
Reply from 192.168.10.201: bytes=32 time<1ms TTL=255
Reply from 192.168.10.201: bytes=32 time=5ms TTL=255

Ping statistics for 192.168.10.201:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 5ms, Average = 1ms

C:\>ping 192.168.10.202

Pinging 192.168.10.202 with 32 bytes of data:

Request timed out.
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255
Reply from 192.168.10.202: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.10.202:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.10.10

Pinging 192.168.10.10 with 32 bytes of data:

Reply from 192.168.10.10: bytes=32 time<1ms TTL=128
Reply from 192.168.10.10: bytes=32 time<1ms TTL=128
Reply from 192.168.10.10: bytes=32 time<1ms TTL=128
Reply from 192.168.10.10: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.10.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
Сетевая связанность со всеми устройствами присутствует.

-----------
### Вопросы для повторения

----------------

1.	С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах, когда было сконфигурировано динамическое обучение - sticky?

* Динамическое (липкое) обучение на порту S2 превращает изученные MAC-адреса в «липкие безопасные» записи (записываются в стартовую конфигурацию), делая их статичными и устраняя необходимость в таймере для старения и удаления этих адресов. Эти записи могут быть удалены вручную


2.	Что касается безопасности порта на S2, если вы загружаете скрипт текущей конфигурации на S2, почему на порту 18  PC-B никогда не получит IP-адрес через DHCP?
* PC-B может не получать IP -адрес через DHCP при загрузке скрипта текущей конфигурации в случае, если не будут указаны (настроены) доверенные (Trusted) порты на коммутаторе.




3.	Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по неактивности?

Устаревание безопасности порта может использоваться для установки времени
устаревания статических и динамических защищенных адресов на порту.

*  Абсолютный - Защищенные адреса порта удаляются по истечении указанного времени
устаревания.
*   По таймеру неактивности -безопасные адреса на порту удаляются, только если они
неактивны в течение указанного времени.



Пример рабочей конфигурации коммутатора SW1 приведен [здесь](/labs/lab09/Config/config%20SW1_9.txt)

Пример рабочей конфигурации коммутатора SW2 приведен [здесь](/labs/lab09/Config/config%20SW2_9.txt)

Пример рабочей конфигурации маршрутизатора R1 приведен [здесь](/labs/lab09/Config/config%20R1_9.txt)


