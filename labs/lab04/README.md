## ДЗ4. Настройка IPv6-адресов на сетевых устройствах.

![](<Топология ДЗ4-1.png>)

## Задачи
1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора.
2. Ручная настройка IPv6-адресов.
3. Проверка сквозного соединения.


## Решение.
### 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора.
-----------------------------------
1. Построим сеть заданной топологии.


![](<Топология ДЗ 4.PNG>)


>Перед начальной конфигурацией необходимо убедиться, что у всех маршрутизаторов и коммутаторов была удалена начальная конфигурация c помощью команды **show startup-config**.  
При необходимости можно удалить файл загрузочной конфигурации командой **erase startup-config**.

2. Проведем настройку базовых параметров сетевых устройств.

  * присваиваем имя;
  * Устанавливаем пароли на консольную и виртуальные линии, на вход в привелегированный режим;
  * шифруем пароли;
  * настраиваем баннер;
  * настраиваем доступ к сетевым устройствам по протоколу **telnet**

  ### 2. Ручная настройка IPv6-адресов.
  ____________

#### Настройка маршрутизатора.  


1. Назначаем IPv6-адреса интерфейсам Ethernet на маршрутизаторе R1.

```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#int gigabitEthernet 0/0/1
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#
R1(config-if)#int gigabitEthernet 0/0/0
R1(config-if)#ipv6 address 2001:db8:acad:a::1/64
R1(config-if)#
R1(config-if)#no shutdown 

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)# 
```
2. Проверяем, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.
Введем команду **show ipv6 interface brief**, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес.
```
R1#
R1#sh ipv6 int br
GigabitEthernet0/0/0       [up/up]
    FE80::290:21FF:FEA4:7001
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::290:21FF:FEA4:7002
    2001:DB8:ACAD:1::1
Vlan1                      [administratively down/down]
    unassigned
R1#
```
Локальные адреса каналов (link-local адреса) назначены на интерфейсах.

>Отображаемый локальный адрес канала основан на адресации EUI-64, которая автоматически использует MAC-адрес интерфейса для создания 128-битного локального IPv6-адреса канала. Характерной особенностью link-local адреса является первый хекстет **FE80:** и вставка в середину адреса блока цифр  **FF:FE**

 Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, необходимо вручную назначить локальные адреса канала на каждом интерфейсе Ethernet на R1. 

>Каждый интерфейс маршрутизатора относится к отдельной сети. Каждый локальный адрес канала должен быть уникальным в пределах одной локальной сети. Пакеты с локальным адресом канала никогда не выходят за пределы локальной сети, а значит, для обоих интерфейсов маршрутизатора можно указывать один и тот же link-local адрес. 

Изменим локальные адреса канала на каждом интерфейсе Ethernet на R1.
```
R1(config)#
R1(config)#int gig0/0/1
R1(config-if)#
R1(config-if)#ipv6 add fe80::1 link-local 
R1(config-if)#
R1(config-if)#
R1(config-if)#int gig0/0/0
R1(config-if)#
R1(config-if)#ipv6 add fe80::1 link-local 
R1(config-if)#^Z
R1#
%SYS-5-CONFIG_I: Configured from console by console
```
Используйте команду **show ipv6 interface brief**, чтобы убедиться, что локальный адрес связи изменен на fe80::1
```
R1#sh ipv6 int br
GigabitEthernet0/0/0       [up/up]
    FE80::1
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::1
    2001:DB8:ACAD:1::1
Vlan1                      [administratively down/down]
    unassigned
R1#
```
3. Активируем IPv6-маршрутизацию на R1.

> По умолчанию IPv6-маршрутизация на ма маршрутизаторах отключена, в отличии от IPv4-маршрутизации. Необходимо активировать IPv6 маршрутизацию.  
Это позволит компьютерам получать IP-адреса и данные шлюза по умолчанию с помощью функции SLAAC (Stateless Address Autoconfiguration (Автоконфигурация без сохранения состояния адреса)).  
Если IPv6 маршрутизация отключена, компьютер PC-B не сможет автоматически получить индивидуальный IPv6-адрес для сетевой интерфейсной карты (NIC).  
 

Активируем IPv6-маршрутизацию на R1 с помощью команды **IPv6 unicast-routing**
```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#
R1(config)#ipv6 unicast-routing
R1(config)#
```



Посмотрим настройки интерфейса GigabitEthernet 0/0/0 R1 после активации IPv6 маршрутизации.

* Интерфейс GigabitEthernet 0/0/0 R1
```
R1#
R1#sh ipv6 int Gig0/0/0
GigabitEthernet0/0/0 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::1
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:A::1, subnet is 2001:DB8:ACAD:A::/64
  Joined group address(es):
    FF02::1
    FF02::2
    FF02::1:FF00:1
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
  ND advertised reachable time is 0 (unspecified)
  ND advertised retransmit interval is 0 (unspecified)
  ND router advertisements are sent every 200 seconds
  ND router advertisements live for 1800 seconds
  ND advertised default router preference is Medium
  Hosts use stateless autoconfig for addresses.
R1#
```
Сохраняем конфигурацию маршрутизатора

* Появилась группа многоадресной рассылки **FF02::2** для всех маршрутизаторов.  
Это группа многоадресной рассылки, в которую включены все IPv6
маршрутизаторы. Маршрутизатор становится частью этой группы,
когда переходит под управление протоколом IPv6 с помощью
команды глобального конфигурирования ipv6 unicast routing.

* Группа многоадресной рассылки для всех узлов **FF02::1**. Это группа
многоадресной рассылки, в которую включены все устройства под
управлением протокола IPv6. Пакет, отправленный этой группе,
принимается и обрабатывается всеми IPv6 интерфейсами в канале или
сети.

* Групповой адрес запрашиваемых узлов **FF02::1:FF00:1** аналогичен групповому адресу для всех узлов. Преимущество группового адреса запрашиваемых узлов заключается в том, что он соответствует специальному адресу многоадресной рассылки Ethernet. Это позволяет сетевой плате Ethernet отфильтровывать кадр, анализируя MAC-адрес назначения без его отправки в IPv6-процесс, чтобы убедиться, что устройство действительно является узлом назначения IPv6-пакета.

#### Настройка коммутатора.

1. Назначаем адрес IPv6 - 2001:db8:acad:1::b/64 для интерфейса VLAN1 SW1. Также назначаем  интерфейсу локальный адрес канала fe80::b.
```
SW1(config)#
SW1(config)#interface vlan 1
SW1(config-if)#ipv6 address 2001:db8:acad:1::b/64
SW1(config-if)#
SW1(config-if)#
SW1(config-if)#ipv6 address fe80::b link-local 
SW1(config-if)#
SW1(config-if)#^Z
SW1#
```
Проверяем правильность назначения IPv6-адресов интерфейсу управления  командой **show ipv6 interface vlan 1**.
```
SW1#show ipv6 interface vlan 1
Vlan1 is up, line protocol is up
  IPv6 is enabled, link-local address is FE80::B
  No Virtual link-local address(es):
  Global unicast address(es):
    2001:DB8:ACAD:1::B, subnet is 2001:DB8:ACAD:1::/64
  Joined group address(es):
    FF02::1
    FF02::1:FF00:B
  MTU is 1500 bytes
  ICMP error messages limited to one every 100 milliseconds
  ICMP redirects are enabled
  ICMP unreachables are sent
  Output features: Check hwidb
  ND DAD is enabled, number of DAD attempts: 1
  ND reachable time is 30000 milliseconds
SW1#
```
 Интерфейсу Vlan 1 назначены указанные IPv6  адреса.

2. Для доступа к коммутатору по протоколу ICMPv6 (проверка сетевой доступности командой **ping** с компьютера PC-B) необходимо на коммутаторе настроить шлюз по умолчанию - прописать статический маршрут по умолчанию к сети 2001:db8:acad:a::/64.
```
SW1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
SW1(config)#
SW1(config)#ipv6 route ::/0 2001:DB8:ACAD:1::1
SW1(config)#
```
Сохраним конфигурацию коммутатора.

#### Настроика компьютеров.

1. Назначаем компьютерам статические IPv6-адреса в соответствии с заданием.

* В окне Свойства Ethernet для каждого ПК и назначаем адресацию IPv6.
* Убеждаемся, что оба компьютера имеют правильную информацию адреса IPv6. Каждый компьютер должен иметь два глобальных адреса IPv6: один статический и один SLACC.


Для компьютера PC-B:
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0001.C758.CE64
   Link-local IPv6 Address.........: FE80::4
   IPv6 Address....................: 2001:DB8:ACAD:A::4
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-A2-62-83-75-00-01-C7-58-CE-64
   DNS Servers.....................: ::
                                     0.0.0.0
```
Для компьютера PC-A:
```
C:\>
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0001.C91A.E04D
   Link-local IPv6 Address.........: FE80::3
   IPv6 Address....................: 2001:DB8:ACAD:1::3
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-AC-68-98-15-00-01-C9-1A-E0-4D
   DNS Servers.....................: ::
                                     0.0.0.0
```
### 3. Проверка сквозного подключения.
_______________

1. С PC-A отправим эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/0/1 на R1.
```
C:\>ping fe80::1

Pinging fe80::1 with 32 bytes of data:

Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255

Ping statistics for FE80::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
Эхо-ответ получен.

2. Отправим эхо-запрос на интерфейс управления SW1 с PC-A.
```
Pinging 2001:db8:acad:1::b with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=255

Ping statistics for 2001:DB8:ACAD:1::B:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
Эхо-ответ получен.

3. Введите команду **tracert** на PC-A, чтобы проверить наличие сквозного подключения к PC-B.
```
C:\>tracert 2001:db8:acad:a::3

Tracing route to 2001:db8:acad:a::3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      2001:DB8:ACAD:1::1
  2   0 ms      0 ms      0 ms      2001:DB8:ACAD:A::3

Trace complete.
```
Трассировка до PC-B показывает наличие сквозного подключения.

4. С PC-B отправим эхо-запрос на PC-A.
```
C:\>ping 2001:db8:acad:1::3

Pinging 2001:db8:acad:1::3 with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:1::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:1::3: bytes=32 time<1ms TTL=127
Reply from 2001:DB8:ACAD:1::3: bytes=32 time<1ms TTL=127

Ping statistics for 2001:DB8:ACAD:1::3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
Эхо-ответ получен.

5. С PC-B отправим эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/0/0 на R1.
```
C:\>ping fe80::1

Pinging fe80::1 with 32 bytes of data:

Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255
Reply from FE80::1: bytes=32 time<1ms TTL=255

Ping statistics for FE80::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>
```
Эхо-ответ получен.

6. с PC-B  отправим эхо-запрос на на интерфейс управления SW1
```
C:\>ping 2001:db8:acad:1::b

Pinging 2001:db8:acad:1::b with 32 bytes of data:

Reply from 2001:DB8:ACAD:1::B: bytes=32 time=2004ms TTL=254
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:1::B: bytes=32 time<1ms TTL=254

Ping statistics for 2001:DB8:ACAD:1::B:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 2004ms, Average = 501ms
```
 Эхо-ответ получен, что показаывет правильную настройку шлюза по умолчанию на коммутаторе SW1. 
______

#### Вопрос для повторения.


1. Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64 ?

Сетевой префикс у IP  адреса **2001:db8:acad::aaaa:1234/64**  - /64, поэтому идентификатор подсети будет составлять 4 хекстета -2001:db8:acad:0, а  идентификатор хоста 4 хекстета - 0:0:aaaa:1234


Файл рабочей конфигурации R1 приведены [здесь](/labs/lab04/Config/Config_R1.txt).

Файл рабочей конфигурации SW1 приведены [здесь](/labs/lab04/Config/Config_SW1.txt).