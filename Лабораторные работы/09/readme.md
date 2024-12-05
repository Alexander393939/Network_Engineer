# Лабораторная работа-Конфигурация безопасности коммутатора

Топология:

![Топология](scrn/Топология.png)

Таблица адресации

|Устройство| interface/vlan | ip-адрес | Маска подсети |
|:------:|:------:|:--------:|:---------:|
|R1| G0/0/1<br>Loopback 0| 192.168.10.1<br>10.10.1.1|255.255.255.0<br>255.255.255.0|
|S1|VLAN 10| 192.168.10.201| 255.255.255.0|
|S2|VLAN 10| 192.168.10.202| 255.255.255.0|
|PC-A|NIC|DHCP|255.255.255.0|
|PC-B|NIC|DHCP|255.255.255.0|

#### Часть 1. Настройка основного сетевого устройства
R1:
```
enable
configure terminal
hostname R1
no ip domain lookup
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202
!
ip dhcp pool Students
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 domain-name CCNA2.Lab-11.6.1
!
interface Loopback0
 ip address 10.10.1.1 255.255.255.0
!
interface GigabitEthernet0/0/1
 description Link to S1
 ip dhcp relay information trust-all
 ip address 192.168.10.1 255.255.255.0
 no shutdown
!
line con 0
 logging synchronous
 exec-timeout 0 0

```
b.	Проверьте текущую конфигурацию на R1


![R1interface](scrn/R1interface.png)

-  Настройка и проверка основных параметров коммутатора
S1:
```
enable
conf t
interface f0/5
description Link R1 G0/0/1

interface f0/6
description Link PC-A

interface F0/1
description Link S2 F0/1

```
S2
```
interface f0/1
description Link S1 F0/1

interface F0/18
description Link PC-B

```
Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих коммутаторах.
```
conf t
ip default-gateway 192.168.10.1
```

#### Часть 2. Настройка сетей VLAN на коммутаторах.

- a. Сконфигруриуйте VLAN 10.
Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.

    

