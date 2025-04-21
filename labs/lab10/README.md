### ДЗ10. Настройка протокола OSPFv2 для одной области.
-----------

![](Топология_10.PNG)

### Задание.

[Часть 1. Создание сети и настройка основных параметров устройства.](README.md#часть-1-создание-сети-и-настройка-основных-параметров-устройства)

[Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области.](README.md#часть-2-настройка-и-проверка-базовой-работы-протокола-ospfv2-для-одной-области)

[Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области.](README.md#часть-3-оптимизация-и-проверка-конфигурации-ospfv2-для-одной-области)

### Решение.
------

#### Общие сведения и сценарий.

* Необходимо настроить сеть небольшой компании с помощью OSPFv2. R1 будет размещать интернет-соединение (имитируемое интерфейсом Loopback 1) и делиться информацией о маршруте по умолчанию до  R2.  
После первоначальной настройки организация попросила оптимизировать конфигурацию, чтобы уменьшить трафик протокола и гарантировать, что R1 продолжает контролировать маршрутизацию.

> Примечание.  
Статическая маршрутизация, используемая в данной лаборатории, заключается в оценке возможности настройки и настройки OSPFv2 в конфигурации для одной области. Этот подход, используемый в данной лаборатории, может не отражать рекомендации по работе с сетевыми сетями. 

### Часть 1. Создание сети и настройка основных параметров устройства/

-----
#### Шаг 1. Создайте сеть согласно топологии.

-----

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

![](Топология_10_1.PNG)


#### Шаг 2. Произведите базовую настройку маршрутизаторов.
-----

Откройте окно конфигурации.  

a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов. 

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

#### Шаг 3. Настройте базовые параметры каждого коммутатора.
-------

a.	Назначьте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

----
### Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области.

------
#### Шаг 1. Настройте адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.

----------

  > Для работы протокола OSPFv2 необходимо настроить IP связанность всех устройств (связанность устройств на L3 уровне)

a.	Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации выше.

Для маршрутизатора R1

```
R1(config)#int gig0/0/1
R1(config-if)#
R1(config-if)#ip address 10.53.0.1 255.255.255.0
R1(config-if)#
R1(config-if)#no shut

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#
R1(config-if)#int loopback 1

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R1(config-if)#
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#
R1(config-if)#
R1(config-if)#exit
R1(config)#
```

Для маршрутизатора R2.
```
R2(config)#int gig0/0/1
R2(config-if)#
R2(config-if)#ip address 10.53.0.1 255.255.255.0
R2(config-if)#
R2(config-if)#no shut

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R2(config-if)#exit
R2(config)#
R2(config)#int loopback 1

R2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R2(config-if)#
R2(config-if)#ip address 172.16.1.1 255.255.255.0
R2(config-if)#
R2(config-if)#
R2(config-if)#exit
R2(config)#exit
R2#
```

Проверим IP связанность узлов сети.
```
R1#
R1#ping 10.53.0.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.53.0.2, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/5/22 ms

R1#ping 10.53.0.3

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.53.0.3, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

R1#ping 10.53.0.4

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.53.0.4, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

R1#ping 172.16.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 2/3/6 ms

R1#
```
IP связанность присутствует со всеми узлами сети.


b.	Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56 и настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для R1, 2.2.2.2 для R2).
```
R1(config)#
R1(config)#
R1(config)#router ospf 56
R1(config-router)#
R1(config-router)#router-id 1.1.1.1
R1(config-router)#
```

```
R2(config)#
R2(config)#
R2(config)#router ospf 56
R2(config-router)#
R2(config-router)#router-id 2.2.2.2
R2(config-router)#
```

с.	Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0.
```
R1(config)#
R1(config)#router os 56
R1(config-router)#
R1(config-router)#network 10.53.0.0 0.0.0.255 area 0
R1(config-router)#
R1(config-router)#exit
R1(config)#
R1(config)#
```

```
R2(config)#
R2(config)#router ospf 56
R2(config-router)#
R2(config-router)#network 10.53.0.0 0.0.0.255 area 0
R2(config-router)#
R2(config-router)#
R2(config-router)#exit
R2(config)#

01:50:16: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done
```


e.	Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область OSPF 0.

```
R2(config)#
R2(config)#int lo1
R2(config-if)#
R2(config-if)#
R2(config-if)#do sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   10.53.0.2       YES manual up                    up 
Loopback1              192.168.1.1     YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
R2(config-if)#ip ospf56 area 0
                     ^
% Invalid input detected at '^' marker.
	
R2(config-if)#ip ospf 56 area 0
R2(config-if)#
R2(config-if)#
R2(config-if)#exit
R2(config)#
R2(config)#
```

f.	Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы убедиться, что R1 и R2 сформировали смежность.

```
R1#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:35    10.53.0.2       GigabitEthernet0/0/1
R1#

R1#
R1#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Backup Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:01
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
```

```
R2#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/BDR        00:00:39    10.53.0.1       GigabitEthernet0/0/1
R2#

R2#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.2/24, Area 0
  Process ID 56, Router ID 2.2.2.2, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Backup Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:01
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 1.1.1.1  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R2#
```

Вопрос:
Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?

* Маршрутизатор R2 является DR, а маршрутизатор R1 - BDR. Критерий выбора DR - наибольший номер идентификатора маршрутизатора R2 (Router-id - 2.2.2.2)


g.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию заключается в объявлении интерфейса обратной связи в качестве маршрута узла с использованием 32-битной маски.

```
R1#
R1#sh ip route ospf
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1 [110/2] via 10.53.0.2, 00:44:10, GigabitEthernet0/0/1

R1#

R1#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.53.0.0/24 is directly connected, GigabitEthernet0/0/1
L       10.53.0.1/32 is directly connected, GigabitEthernet0/0/1
     172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
C       172.16.1.0/24 is directly connected, Loopback1
L       172.16.1.1/32 is directly connected, Loopback1
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1/32 [110/2] via 10.53.0.2, 00:06:22, GigabitEthernet0/0/1
R1#
```


h.	Запустите Ping до  адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно быть успешным.

```
R1#ping 192.168.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/3/13 ms

R1#
```
### Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области

-----

#### Шаг 1. Реализация различных оптимизаций на каждом маршрутизаторе.

a.	На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является назначенным маршрутизатором.
```
RR1(config)#
R1(config)#int g0/0/1
R1(config-if)#
R1(config-if)#ip ospf priority 50
R1(config-if)#
R1(config-if)#^Z
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#clear ip ospf process 
Reset ALL OSPF processes? [no]: y

R1#
04:54:57: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

04:54:57: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R1#
04:54:57: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R1#
R1#
R1#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:33    10.53.0.2       GigabitEthernet0/0/1
R1#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:08
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
R1#
```

* Маршрутизатор R1 стал DR, а маршрутизатор R2 - BDR из-за увеличения приоритета OSPF интерфейса G0/0/1 на R1.

b.	Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия, составляющего 30 секунд.

------

>По умолчанию на маршрутизаторах R1 и R2 длительность hello-интервалов равна 10 с, длительность dead-интервалов - 40 с. Изменим вручную длительность hello-интервалов и длительность dead-интервалов. Изменять длительности интервалов нужно на всех маршрутизаторах области ospf.  
Рекомендуется изменять таймеры OSPF, чтобы маршрутизаторы
быстрее могли обнаружить сбои в сети. Это увеличивает трафик, но
иногда важнее обеспечить быструю сходимость, чем экономить на
трафике.

```
R1(config)#
R1(config)#int g0/0/1
R1(config-if)#
R1(config-if)#
R1(config-if)#ip ospf hello-interval 30
R1(config-if)#
R1(config-if)#
R1(config-if)#
05:22:10: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Dead timer expired

05:22:10: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R1(config-if)#do sh ip ospf int

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  No backup designated router on this network
  Timer intervals configured, Hello 30, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:01
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
R1(config-if)#ip ospf dead-interval 120
R1(config-if)#
R1(config-if)#do sh ip ospf int

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  No backup designated router on this network
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    Hello due in 00:00:21
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
R1(config-if)#
R1(config-if)#
05:25:50: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R1(config-if)#
```

* После изменения длительности hello-интервалов и dead-интервалов на маршрутизаторе R1 процесс OSPF остановился. После аналогичной настройки длительности hello-интервалов и dead-интервалов на маршрутизаторе R2 процесс OSPF вновь поднялся. Это говорит о том, что длительности интервалов должны быть одинаковы на всех маршрутизаторах области OSPF.


c.	На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите внимание на сообщение консоли после установки маршрута по умолчанию.

------
```
R1#
R1#sh ip int br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   10.53.0.1       YES NVRAM  up                    up 
Loopback1              172.16.1.1      YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#
R1(config)#int lo1
R1(config-if)#
R1(config-if)#ip route 0.0.0.0 0.0.0.0 lo1
%Default route without gateway, if not a point-to-point interface, may impact performance
R1(config)#
R1(config)#router ospf 56
R1(config-router)#
R1(config-router)#default-information originate 
R1(config-router)#
R1(config-router)#end
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#
R1#
R1#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is 0.0.0.0 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.53.0.0/24 is directly connected, GigabitEthernet0/0/1
L       10.53.0.1/32 is directly connected, GigabitEthernet0/0/1
     172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
C       172.16.1.0/24 is directly connected, Loopback1
L       172.16.1.1/32 is directly connected, Loopback1
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1/32 [110/2] via 10.53.0.2, 00:24:44, GigabitEthernet0/0/1
S*   0.0.0.0/0 is directly connected, Loopback1

R1#
```
Посмотрим таблицу маршрутизации на R2
```
R2#
R2#sh ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is 10.53.0.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.53.0.0/24 is directly connected, GigabitEthernet0/0/1
L       10.53.0.2/32 is directly connected, GigabitEthernet0/0/1
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, Loopback1
L       192.168.1.1/32 is directly connected, Loopback1
O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:03:55, GigabitEthernet0/0/1

R2#
```

* Как видно из листинга команды, на маршрутизаторе R2 появился новый маршрут по умолчанию - **O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:03:55, GigabitEthernet0/0/1**.  
Маршрут по умолчанию настраивается и распространяется на
все другие маршрутизаторы OSPF в домене маршрутизации.

d.	добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети интерфейса.

-----
```
R2#
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#
R2(config)#
R2(config)#int lo1
R2(config-if)#
R2(config-if)#ip ospf 56 area 0
R2(config-if)#
R2(config-if)#ip ospf network po
R2(config-if)#ip ospf network point-to-point 
R2(config-if)#
R2(config-if)#
R2(config-if)#do sh ip ospf int lo 1

Loopback1 is up, line protocol is up
  Internet address is 192.168.1.1/24, Area 0
  Process ID 56, Router ID 2.2.2.2, Network Type POINT-TO-POINT, Cost: 1
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
  Index 2/2, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Suppress hello for 0 neighbor(s)
R2(config-if)#exit
R2(config)#
```


e.	Только на R2 добавьте конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.

-----
```
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#
R2(config)#router ospf 56
R2(config-router)#
R2(config-router)#passive-interface lo1
R2(config-router)#
R2(config-router)#end
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#
R2#sh ip prot
R2#sh ip protocols 

Routing Protocol is "ospf 56"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 2.2.2.2
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    10.53.0.0 0.0.0.255 area 0
  Passive Interface(s): 
    Loopback1
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    1.1.1.1              110      00:02:31
    2.2.2.2              110      00:08:14
  Distance: (default is 110)

R2#
```


f.	Измените базовую пропускную способность для маршрутизаторов. После этой настройки перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на сообщение консоли после установки новой опорной полосы пропускания.

------

На маршрутизаторе R1/
```
R1#
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#
R1(config)#router ospf 56
R1(config-router)#
R1(config-router)#auto-cost  reference-bandwidth 10000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R1(config-router)#
R1(config-router)#^Z
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#clear
R1#clear ip ospf pro
R1#clear ip ospf process 
Reset ALL OSPF processes? [no]: y

R1#
07:09:05: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

07:09:05: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R1#
07:09:21: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R1#
07:09:56: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R1#
R1#
```

На маршрутизаторе R2

```
R2#
R2#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#
R2(config)#router ospf 56
R2(config-router)#
R2(config-router)#auto-cost  reference-bandwidth 10000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R2(config-router)#
R2(config-router)#
R2(config-router)#^Z
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#conf t 
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#router ospf 56
07:09:21: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R2(config)#^Z
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#clear ip ospf process
Reset ALL OSPF processes? [no]: y

R2#
07:09:36: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

07:09:36: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R2#
07:09:56: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

R2#
R2#
```
После презапуска процесса OSPF  стоимость изменилась.Была равна 1, теперь стоимость составляет 100.
```
R1#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 100
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    Hello due in 00:00:08
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
````
```
R2#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.2/24, Area 0
  Process ID 56, Router ID 2.2.2.2, Network Type BROADCAST, Cost: 100
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    Hello due in 00:00:11
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 1.1.1.1  (Designated Router)
  Suppress hello for 0 neighbor(s)
R2#
```

#### Шаг 2. Убедитесь, что оптимизация OSPFv2 реализовалась.

-----


a.	Выполните команду  **show ip ospf interface g0/0/1**  на R1 и убедитесь, что приоритет интерфейса установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию — Broadcast.
```
R1#
R1#sh ip ospf int g0/0/1

GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 100
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    Hello due in 00:00:11
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
R1#
```

b.	На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1 присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим выходным и предыдущим выходным. Также обратите внимание, что маска теперь составляет 24 бита, в отличие от 32 битов, ранее объявленных.

```
R1#
R1#show ip route ospf
O    192.168.1.0 [110/101] via 10.53.0.2, 00:22:19, GigabitEthernet0/0/1

R1#
```

c.	Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.
```
R2#show ip route ospf
O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:25:25, GigabitEthernet0/0/1

R2#
```

d.	Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно быть успешным.
```
R2#
R2#ping 172.16.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

R2#
```
Отработал распростаняемый маршрут по умолчанию.



### Вопрос:
Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24?

>Cтоимость OSPF для маршрута по умолчанию составляет 1, стоимость OSPF в R1 для сети 192.168.1.0/24 - 101. Это отличие объясняется тем, что стоимость  маршрута OSPF в R1 для сети 192.168.1.0/24 складывается из стоимости маршрута до R2 (100) и стимости распространяемого маршрута по умолчанию - интерфейс Lo1 ( 1 ).


Пример рабочей конфигурации маршрутизатора R1 приведен [здесь](/labs/lab10/Configs/ConfigR1_10.txt)

Пример рабочей конфигурации маршрутизатора R2 приведен [здесь](/labs/lab10/Configs/ConfigR2_10.txt)









