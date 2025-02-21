##  ДЗ 6. Внедрение маршрутизации между виртуальными локальными сетями.

![](Топология_6.PNG)

Немного изменим заданную топологию с целью лучшего анализа работы сети с использованием технологии виртуальных локальных сетей.

Добавим по одному хосту на коммутатор таким образом, чтобы на каждои коммутаторе присутствовали хосты разных VLAN-ов.  
 Кроме того добавим один хост в VLAN Управления. Это позволит иметь доступ к управлению коммутаторами в случае аварии с внутресетевой маршрутизацией.

![](Топология_6-1.PNG)

Твблица адресации.

| Устройство | Интерфейс    | IP-адрес    |Маска подсети | Шлюз по умолчанию|
|:----------:|:------------:|:-----------:|:------------:|:----------------:|
|  R1        | G0/0/1.10    | 192.168.10.1 |255.255.255.0|  -----           |
|  R1        | G0/0/1.20    | 192.168.20.1 |255.255.255.0|  -----           |
|  R1        | G0/0/1.30    | 192.168.10.1 |255.255.255. |  -----           |
|  R1        | G0/0/1.1000  | -----        |  -----      |  -----           |
|  SW1       | VLAN 10      | 192.168.10.11|255.255.255.0|192.168.10.1      |
|  SW2       | VLAN 10      | 192.168.20.12|255.255.255.0|192.168.10.1      |
|  PC1       | NIC          | 192.168.20.3 |255.255.255.0|192.168.20.1      |
|  PC2       | NIC          | 192.168.30.3 |255.255.255.0|192.168.30.1      |
|  PC3       | NIC          | 192.168.30.2 |255.255.255.0|192.168.30.1      |
|  PC4       | NIC          | 192.168.20.2 |255.255.255.0|192.168.20.1      |
|  PC5       | NIC          | 192.168.10.10 |255.255.255.0|192.168.10.1     |


Таблица VLAN

| VLAN | Имя   | Назначенный интерфейс    |
|:----------:|:------------:|:-----------:|
|  10        |Управление    | SW1: VLAN 10 , SW2: VLAN 10 , SW2: F0/3|
|  20        | Sales        | SW1: F0/2 , SW2: F0/1|
|  30        | Operations   | SW1: F0/1 , SW2: F0/2|
|  999       | Parking_Lot  | SW1: F0/3-24 , SW2: F0/4-24, SW2: G0/2|
|  1000      | Собственная  | -----        |

## Задачи
1. Создание сети и настройка основных параметров устройства.
2. Создание сетей VLAN и назначение портов коммутатора.
3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами.

4. Настройка маршрутизации между сетями VLAN.
5. Проверка работы маршрутизации между VLAN.

## Решение.
### 1. Создание сети и настройка основных параметров устройства.
-----------------------------------

Шаг 1.  Построим сеть заданной топологии. 

Шаг 2. Проведем настройку базовых параметров маршрутизатора R1.

* присваиваем имя;
* присваиваем доменное имя и создаем локального пользователя.
* Устанавливаем пароли на вход по консольной линии, вход на виртуальные линиии по протоколу SSH  и запрещаем вход по линии AUX;
* шифруем пароли, настраиваем предотвращение атаки с использованием пароля грубой силы;
* настраиваем баннер;
* настраиваем время на маршрутизаторе кмандой **clock set**.

Шаг 3. Проведем настройку базовых параметров коммутаторов SW1 и SW2.

* присваиваем имя;
* присваиваем доменное имя и создаем локального пользователя.
* Устанавливаем пароли на вход по консольной линии, вход на виртуальные линиии по протоколу SSH;
* шифруем пароли, настраиваем предотвращение атаки с использованием пароля грубой силы;
* настраиваем баннер;
* настраиваем время на коммутаторах кмандой **clock set**.

Шаг 4. Настраиваем узлы ПК.

Адреса ПК можно посмотреть в таблице адресации (см. выше).

### 2. Создание сетей VLAN и назначение портов коммутатора
-----------------
>Cоздадим VLAN, как указано в таблице выше, на обоих коммутаторах. Затем  назначим VLAN соответствующему интерфейсу и проверим настройки конфигурации. 

Шаг 1. Создайте сети VLAN на коммутаторах.

* a.	Создадим и присвоим имена необходимым VLAN на каждом коммутаторе из таблицы выше.
~~~
SW1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1(config)#
SW1(config)#vlan 10
SW1(config-vlan)#
SW1(config-vlan)#name Control
SW1(config-vlan)#exit
SW1(config)#vlan 20
SW1(config-vlan)#name Sales
SW1(config-vlan)#exit
SW1(config)#vlan 30
SW1(config-vlan)#
SW1(config-vlan)#name Operation
SW1(config-vlan)#exit
SW1(config)#vlan 999
SW1(config-vlan)#
SW1(config-vlan)#name Parking_Lot
SW1(config-vlan)#exit
SW1(config)#vlan 1000
SW1(config-vlan)#
SW1(config-vlan)#name Native
SW1(config-vlan)#exit
SW1(config)
~~~~
На коммутаторе SW2 сделаем аналогичные настройки VLAN.



* Настроим интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

Коммутатор SW1
~~~
SW1(config)#
SW1(config)#ip default-gateway 192.168.10.1
SW1(config)#int vlan 10
SW1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up

SW1(config-if)#ip address 192.168.10.11 255.255.255.0
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#
~~~

Коммутатор  SW2
~~~
SW2(config)#
SW2(config)#ip default-gateway 192.168.10.1
SW2(config)#int vlan 10
SW2(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up

SW2(config-if)#ip address 192.168.10.12 255.255.255.0
SW2(config-if)#
SW2(config-if)#exit
SW2(config)#
~~~

* Назначим все неиспользуемые порты коммутатора VLAN Parking_Lot, настроим их для статического режима доступа и административно деактивируем их.

~~~
SW1(config)#
SW1(config)#
SW1(config)#int range fa0/1-24
SW1(config-if-range)#
SW1(config-if-range)#
SW1(config-if-range)#switchport mode access
SW1(config-if-range)#
SW1(config-if-range)#
SW1(config-if-range)#exit
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#int range fa0/3-24
SW1(config-if-range)#switchport access vlan 999
SW1(config-if-range)#
SW1(config-if-range)#shut
SW1(config-if-range)#exit
SW1(config)#
~~~

Шаг 2. Назначим используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настроим их для режима статического доступа.
~~~

SW1(config)#int fa0/1
SW1(config-if)#
SW1(config-if)#switchport access vlan 30
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#
SW1(config)#int fa0/2
SW1(config-if)#
SW1(config-if)#switchport access vlan 20
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#
~~~
* посмотрим таблицу VLAN  на коммутаторe SW1.

~~~
SW1(config)#do sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gig0/1, Gig0/2
10   Control                          active    
20   Sales                            active    Fa0/2
30   Operation                        active    Fa0/1
999  Parking_Lot                      active    Fa0/3, Fa0/4, Fa0/5, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/18
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24
1000 Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
~~~



* Проведем аналогичные настройки на коммутаторе SW2  с учетом используемых портов коммутатора SW2.

посмотрим таблицу VLAN  на коммутаторe SW2.
~~~
SW2#sh vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gig0/1
10   Control                          active    Fa0/3
20   Sales                            active    Fa0/1
30   Operation                        active    Fa0/2
999  Parking_Lot                      active    Fa0/4, Fa0/5, Fa0/6, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/2
1000 Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
~~~


### 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами.
---------------------

Шаг 1. Вручную настроим магистральный интерфейс G0/1 на коммутаторах SW1 и SW2.

* Настройка статического транкинга и нативного VLAN на интерфейсе G0/1 для обоих коммутаторов.

~~~
SW1(config)#
SW1(config)#int gig0/1
SW1(config-if)#switchport mode trunk
SW1(config-if)#
SW1(config-if)#switchport trunk native vlan 1000
SW1(config-if)#
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#
~~~
* Указываем, что VLAN 10, 20, 30 и 1000 могут проходить по транку и отключаем Протокол динамического транкинга (DTP).
~~~
SW1(config)#
SW1(config)#int gig0/1
SW1(config-if)#
SW1(config-if)#switchport trunk allowed vlan 10,20,30,1000
SW1(config-if)#
SW1(config-if)#switchport nonegotiate
SW1(config-if)#
SW1(config-if)#exit
SW1(config)#
~~~
При этом транк будет недоступен для VLAN 999 и VLAN 1.


Шаг 2. Вручную настроим магистральный интерфейс G0/2 на коммутаторе SW1. Это будет транк до маршрутизатора.

* Настройки транка на интерфейсе G0/2 коммутатора SW1 аналогичны настройкам интерфейса G0/1.












