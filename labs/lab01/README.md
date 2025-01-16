## ДЗ1. Базовая настройка коммутатора
>
![](Lab1.png)

##	Задачи
 1. Проверка конфигурации коммутатора по умолчанию.
 2. Создание сети и настройка основных параметров устройства.
    * Настройте базовые параметры коммутатора.
    * Настройте IP-адрес для ПК.
 3. Проверка сетевых подключений

    * Отобразите конфигурацию устройства.
    * Протестируйте сквозное соединение, отправив эхо-запрос.
    * Протестируйте возможности удаленного управления с помощью Telnet.



## Решение
### 1. Создание сети и проверка конфигурации коммутатора по умолчанию.
      
 Шаг 1. Создаим сеть согласно топологии в задании.

 
Топология

![](Топология.png)

* Подсоединяю консольный кабель.
*	Устанавливаю консольное подключение к коммутатору с компьютера PC-A
 
> Для первоначальной настройки коммутатора нужно использовать консольное подключение потому, что коммутатор изначально не настроен для внешнего подключения. Именно поэтому невозможно подключится ни по Telnet протоколу, ни по SSH.
> 
Шаг 2. Проверяем настройки коммутатора по умолчанию.

На данном этапе проверяю такие параметры коммутатора по умолчанию, как текущие настройки коммутатора, данные IOS, свойства интерфейса, сведения о VLAN и флеш-память.

* Подключаюсь к коммутатору с помощью консоли и вхожу в привилегированный режим EXEC. Командой **show flash**, определяю, были ли созданы сети VLAN на коммутаторе.

```
Switch> enable
Switch#
Switch#sh flash
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin
```
  Файла vlan.dat нет. Сети VLAN не создавались. При этом видно 

  * Проверяем наличие файла загрузочной конфигурации и содержимое конфигурации по умолчанию.

```
Switch#sh start
startup-config is not present
Switch#
Switch#sh run
```
Файл загрузочной конфигурации не создавался. А файл конфигурации по умолчанию пустой.

```
Switch#sh run
Building configuration...

Current configuration : 1080 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Switch
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
!
!
!
!
line con 0
!
line vty 0 4
 login
line vty 5 15
 login
!
!
!
!
end
```
При необходимости можно удалить файл загрузочной конфигурации командой **erase startup-config**.

> Из листинга файла конфигурации по умолчанию **running configuration** можно получить следующую информацию:
> * На коммутаторе имеется 24 интерфейса FastEthernet;
> * На коммутаторе имеется 2 интерфейса Gigabit Ethernet;
> * Диапазон значений, отображаемых в vty-линиях -16 (с 0 по 15)
> * Под управлением какой версии ОС Cisco IOS работает коммутатор;
> * Характеристики и состояние SVI для VLAN 1. В данном случае видно , что SVI для VLAN 1 выключен и ему не назначен IP адрес.
 

### 2. . Настройка базовых параметров сетевых устройств/

Во второй части необходимо будет настроить основные параметры коммутатора и компьютера.


Шаг 1. Настрою базовые параметры коммутатора.