## ДЗ3. Расчет подсетей IPv4.

## Задачи
1. Определение подсетей по IPv4-адресу.
2. Расчет подсетей по IPv4-адресу.

## Решение.
>Умение работать с IPv4-подсетями и определять информацию о сетях и узлах на основе известного IP-адреса и маски подсети необходимо для понимания принципов работы IPv4-сетей. Цель первой части — закрепить знания о том, как рассчитывать IP-адрес сети на основе известного IP-адреса и маски подсети. Зная IP-адрес и маску подсети, вы всегда сможете получить другие данные об этой подсети.  
>Цель второй части - получить практические навыки расчета подсетей на основе известного IP-адреса и маски подсети.

### 1. Определение подсетей по IPv4-адресу.

_______________
### Задание 1
________________

Дано


| | |
|-------------------------|:-----------------:|
| IP-адрес узла:          | 192.168.200.139 |
| Исходная маска подсети: | 255.255.255.0   |
| Новая маска подсети:    | 255.255.255.224 |

Найти

| | |
|---------------------------------------------|:-----------------:|
| Количество бит подсети                      | 3               |
| Количество созданных подсетей               | 8               |
| Количество бит узлов в подсети              | 5               |
| Количество узлов в подсети                  | 30              |
| Сетевой адрес этой подсети                  | 192.168.200.128 |
| IPv4-адрес первого узла в этой подсети      | 192.168.200.129 |
| IPv4-адрес последнего узла в этой подсети   | 192.168.200.158 |
| Широковещательный IPv4-адрес в этой подсети | 192.168.200.159 |


  >Для перевода чисел из двоичной в десятичную системы счисления и обратно, воспользуемся шаблоном

###  Шаблон  для перевода 

|         Степень числа 2 | $2^7$   | $2^6$  | $2^5$  | $2^4$  | $2^3$ | $2^2$ | $2^1$ | $2^0$ |
|-------------------------|---------|--------|--------|--------|-------|-------|-------|-------|
| **Десятичное число**    | **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** |

 + **Определим исходную сеть, наложив исходную маску на IP адрес.**

| | | |
|-------------------------|:----------------:|:-----------------|
| IP-адрес узла:          | 192.168.200.139  | 11000000.10101000.11001000.10001011
| Исходная маска подсети: | 255.255.255.0    | 11111111.11111111.11111111.00000000
| **Исходная сеть**       |**192.168.200.0** |**11000000.10101000.11001000.00000000**|
  + **Обозначим новую маску для сегментации сети.**

Новая маска - 255.255.255.224 - 	11111111.11111111.11111111.**111**_00000    -  /27  
Таким образом количество бит, забираемых из адреса хоста для сегментации сети  - 3
Количество бит для адреса хоста – 5


  + **Определим количество создаваемых сетей и количество хостов в сети.**
  
     Количество создаваемых сетей с новой маской вычисляется по формуле:

      $N=2^k$  
      N – количество сетей;  
      К – количество бит, предоставленных для адреса сети;

      $N=2^3$= 8 сетей

      Количество узлов в сети вычисляется по формуле:

    $H=2^N$-2  
      H - количество хостов сети;        
      К – количество бит, предоставленных для адреса сети;  

    $H=2^5$-2= 30 хостов

### 2. Расчет подсетей по IPv4-адресу.
  
  +	**Определим новые сети.**

  >Исходная сеть – 192.168.200.0 /24 делится на  8 подсетей с помощью новой маски  /27  
  >Последний октет в IP адресе для визуализации расчетов отобразим в двоичном виде.

+ #### 1 подсеть –	192.168.200.00000000 	- 192.168.200.0 /27

| | | |
|----------------------------|:--------------------:|:--------------|
| IP- адрес сети             | 192.168.200.00000000 | 192.168.200.0 |
| IP- адрес первого узла     | 192.168.200.00000001 | 192.168.200.1 |
| IP- адрес последнего узла  | 192.168.200.00011110 |192.168.200.30 |
|Широковещательный адрес     | 192.168.200.00011111 |192.168.200.31 |

+ #### 2 подсеть –	192.168.200.00100000 – 192.168.200.32 /27

| | | |
|----------------------------|:--------------------:|:---------------|
| IP- адрес сети             | 192.168.200.00100000 | 192.168.200.32 |
| IP- адрес первого узла     | 192.168.200.00100001 | 192.168.200.33 |
| IP- адрес последнего узла  | 192.168.200.00111110 | 192.168.200.62 |
|Широковещательный адрес     | 192.168.200.00111111 | 192.168.200.63 |

+ #### 3 подсеть –	192.168.200.01000000 – 192.168.200.64 /27

| | | |
|----------------------------|:--------------------:|:---------------|
| IP- адрес сети             | 192.168.200.01000000 | 192.168.200.64 |
| IP- адрес первого узла     | 192.168.200.01000001 | 192.168.200.65 |
| IP- адрес последнего узла  | 192.168.200.01011110 | 192.168.200.94 |
|Широковещательный адрес     | 192.168.200.01011111 | 192.168.200.95 |

+ #### 4 подсеть –	192.168.200.01100000 – 192.168.200.96 /27

| | | |
|----------------------------|:--------------------:|:----------------|
| IP- адрес сети             | 192.168.200.01100000 | 192.168.200.96  |
| IP- адрес первого узла     | 192.168.200.01100001 | 192.168.200.97  |
| IP- адрес последнего узла  | 192.168.200.01111110 | 192.168.200.126 |
|Широковещательный адрес     | 192.168.200.01111111 | 192.168.200.127 |


+ #### 5 подсеть –	192.168.200.10000000 – 192.168.200.128 /27
| | | |
|----------------------------|:--------------------:|:----------------|
| IP- адрес сети             | 192.168.200.10000000 | 192.168.200.128 |
| IP- адрес первого узла     | 192.168.200.10000001 | 192.168.200.129 |
| IP- адрес последнего узла  | 192.168.200.10011110 | 192.168.200.158 |
|Широковещательный адрес     | 192.168.200.10011111 | 192.168.200.159 |

+ #### 6 подсеть –	192.168.200.10100000 – 192.168.200.160 /27

| | | |
|----------------------------|:--------------------:|:----------------|
| IP- адрес сети             | 192.168.200.10100000 | 192.168.200.160 |
| IP- адрес первого узла     | 192.168.200.10100001 | 192.168.200.161 |
| IP- адрес последнего узла  | 192.168.200.10111110 | 192.168.200.190 |
|Широковещательный адрес     | 192.168.200.10111111 | 192.168.200.191 |

+ #### 7 подсеть –	192.168.200.11000000 – 192.168.200.192 /27

| | | |
|----------------------------|:--------------------:|:----------------|
| IP- адрес сети             | 192.168.200.11000000 | 192.168.200.192 |
| IP- адрес первого узла     | 192.168.200.11000001 | 192.168.200.193 |
| IP- адрес последнего узла  | 192.168.200.11011110 | 192.168.200.222 |
|Широковещательный адрес     | 192.168.200.11011111 | 192.168.200.223 |

+ #### 8 подсеть –	192.168.200.11100000 – 192.168.200.224 /27

| | | |
|----------------------------|:--------------------:|:----------------|
| IP- адрес сети             | 192.168.200.11100000 | 192.168.200.224 |
| IP- адрес первого узла     | 192.168.200.11100001 | 192.168.200.225 |
| IP- адрес последнего узла  | 192.168.200.11111110 | 192.168.200.254 |
|Широковещательный адрес     | 192.168.200.11111111 | 192.168.200.255 |


_________________
### Задание 2
_________________

| | |
|-------------------------|:-------------:|
| IP-адрес узла:          | 10.101.99.228 |
| Исходная маска подсети: | 255.0.0.0     |
| Новая маска подсети:    | 255.255.128.0 |

Найти

| | |
|---------------------------------------------|:--------------:|
| Количество бит подсети                      | 9              |
| Количество созданных подсетей               | 512            |
| Количество бит узлов в подсети              | 15             |
| Количество узлов в подсети                  | 32766          |
| Сетевой адрес этой подсети                  | 10.101.0.0     |
| IPv4-адрес первого узла в этой подсети      | 10.101.0.1     |
| IPv4-адрес последнего узла в этой подсети   | 10.101.127.254 |
| Широковещательный IPv4-адрес в этой подсети | 10.101.127.255 |

_________________
### Задание 3
_________________

| | |
|-------------------------|:-------------:|
| IP-адрес узла:          | 172.22.32.12  |
| Исходная маска подсети: | 255.255.0.0   |
| Новая маска подсети:    | 255.255.254.0 |

Найти

| | |
|---------------------------------------------|:-------------:|
| Количество бит подсети                      | 3             |
| Количество созданных подсетей               | 8             |
| Количество бит узлов в подсети              | 13            |
| Количество узлов в подсети                  | 8190          |
| Сетевой адрес этой подсети                  | 172.22.32.0   |
| IPv4-адрес первого узла в этой подсети      | 172.22.32.1   |
| IPv4-адрес последнего узла в этой подсети   | 172.22.63.254 |
| Широковещательный IPv4-адрес в этой подсети | 172.22.63.255 |

_________________
### Задание 4
_________________

| | |
|-------------------------|:---------------:|
| IP-адрес узла:          | 192.168.1.245   |
| Исходная маска подсети: | 255.255.255.0   |
| Новая маска подсети:    | 255.255.255.252 |

Найти

| | |
|---------------------------------------------|:-------------:|
| Количество бит подсети                      | 6             |
| Количество созданных подсетей               | 64            |
| Количество бит узлов в подсети              | 2             |
| Количество узлов в подсети                  | 2             |
| Сетевой адрес этой подсети                  | 192.168.1.244 |
| IPv4-адрес первого узла в этой подсети      | 192.168.1.245 |
| IPv4-адрес последнего узла в этой подсети   | 192.168.1.246 |
| Широковещательный IPv4-адрес в этой подсети | 192.168.1.247 |

_________________
### Задание 5
_________________

| | |
|-------------------------|:-------------:|
| IP-адрес узла:          | 128.107.0.55  |
| Исходная маска подсети: | 255.255.0.0   |
| Новая маска подсети:    | 255.255.255.0 |

Найти

| | |
|---------------------------------------------|:-------------:|
| Количество бит подсети                      | 8             |
| Количество созданных подсетей               | 256           |
| Количество бит узлов в подсети              | 8             |
| Количество узлов в подсети                  | 254           |
| Сетевой адрес этой подсети                  | 128.107.0.0   |
| IPv4-адрес первого узла в этой подсети      | 128.107.0.1   |
| IPv4-адрес последнего узла в этой подсети   | 128.107.0.254 |
| Широковещательный IPv4-адрес в этой подсети | 128.107.0.255 |

_________________
### Задание 6
_________________

| | |
|-------------------------|:---------------:|
| IP-адрес узла:          | 192.135.250.180 |
| Исходная маска подсети: | 255.255.255.0   |
| Новая маска подсети:    | 255.255.255.248 |

Найти

| | |
|---------------------------------------------|:---------------:|
| Количество бит подсети                      | 5               |
| Количество созданных подсетей               | 32              |
| Количество бит узлов в подсети              | 3               |
| Количество узлов в подсети                  | 6               |
| Сетевой адрес этой подсети                  | 192.135.250.176 |
| IPv4-адрес первого узла в этой подсети      | 192.135.250.177 |
| IPv4-адрес последнего узла в этой подсети   | 192.135.250.182 |
| Широковещательный IPv4-адрес в этой подсети | 192.135.250.183 |



>Почему маска подсети так важна при анализе IPv4-адреса?  
Маска подсети позволяет разделить IP адрес на номер сети и номер хоста. Маска показывает, сколько битов включает в себя номер сети, Кроме того с помощью масок можно разделять большие сети на более мелкие. Еще одна важная функция масок - указывать диапазон IP-адресов, принадлежащих одной сети. Таким образом, можно управлять сетевым трафиком и настраивать сетевые устройства, например, маршрутизаторы и коммутаторы.
