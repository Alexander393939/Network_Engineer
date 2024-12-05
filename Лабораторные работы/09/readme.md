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

 ```
 conf t
 vlan 10
 name MNG
 ```   
- Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 и S2. Включите интерфейсы SVI и предоставьте описание для интерфейса.

S1:
```
conf t
interface vlan 10
ip address 192.168.10.201 255.255.255.0
description S1 MNG VLAN
```
S2:
```
conf t
interface vlan 10
ip address 192.168.10.202 255.255.255.0
description S2 MNG VLAN
```
- Настройте VLAN 333 с именем Native на S1 и S2.
- Настройте VLAN 999 с именем ParkingLot на S1 и S2.
```
conf t
vlan 333
name native
vlan 999
name Parking_lot
```
#### Часть 3. Настройки безопасности коммутатора.

- Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в качестве native VLAN.

```
conf t
interface f0/1
switchport mode trunk
switchport trunk native vlan 333
```
![S1trunk333](scrn/S1trunk333.png)
![S2trunk333](scrn/S2trunk333.png)

- Отключить согласование DTP F0/1 на S1 и S2.
```
conf t
interface f0/1
switchport nonegotiate
```
![DTP](scrn/DTP.png)\

- На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10.

```
conf t
interface range f0/5-6
switchport mode access

```
- На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10.
```
conf t
interface f0/18
switchport mode access
```


