### ДЗ10. Настройка протокола OSPFv2 для одной области.
-----------

![](Топология_10.PNG)

### Задание.

Часть 1. Создание сети и настройка основных параметров устройства.

Часть 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области.

Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области.

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
R1#
R1#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:32    10.53.0.2       GigabitEthernet0/0/1
R1#
```
```
R2#
R2#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/DR         00:00:31    10.53.0.1       GigabitEthernet0/0/1
R2#
```

Вопрос:
Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии отбора?

* Маршрутизатор R2 является DR. Критерий выбора DR - наибольший номер идентификатора маршрутизатора (Router-id - 2.2.2.2)


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









