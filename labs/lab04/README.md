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


![](<Топология ДЗ 4.png>)


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













