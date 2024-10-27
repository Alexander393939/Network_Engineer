### 06 Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями

#### Топология:
![Топология](scrn/Топология.png)

#### Таблица адресации:

|Устройства|Интерфейс|IP-адрес|Маска подсети| Шлюз по умолчанию 
|:--------------:|:------------:|:-----------|:-----------:|:------------:|
R1|G0/0/1.10<br/>G0/0/1.20<br/>G0/0/1.30<br/>G0/0/1.1000|192.168.10.1<br/>192.168.20.1<br/>192.168.30.1<br/>-------------|255.255.255.0<br/>255.255.255.0<br/>255.255.255.0<br/>--------------|-----------|
S1|VLAN10|192.168.10.11|255.255.255.0|192.168.10.1|
S2|VLAN10|192.168.10.12|255.255.255.0|192.168.10.1|
PC-A|NIC|192.168.20.3|255.255.255.0|192.168.20.1|
PC-B|NIC|192.168.30.3|255.255.255.0|192.168.30.1|

#### Таблица VLAN:
|VLAN|ИМЯ|НАЗНАЧЕННЫЙ ИНТЕРФЕЙС
|:----------------:|:----------------:|:---------------------------------------:|
10|Управление|S1:VLAN 10<br/>S2:VLAN 10|
20|Sales|S1: F0/6|
30|Operations|S2: F0/18|
999|Parking_Lot|C1: F0/2-4, F0/7-24, G0/1-2<br/>C2: F0/2-17, F0/19-24, G0/1-2|

#### Часть 1. Создание сети и настройка основных параметров устройства :

Шаг 1. Создайте сеть согласно топологии.
Подключите устройства, как показано в топологии, и подсоедините необходимые кабели:

![схема](scrn/схема.png)

**Шаг 2. Настройте базовые параметры для маршрутизатора и коммутаторов .**<br/>
```
enable
configure terminal
hostname R1
no ip domain-lookup
```
**Настройка паролей:**
```
conf t
enable secret class
line console 0
password cisco
login
exit
line vty 0 4
login
exit
service password-encryption
```
**Создать баннер:**
```
conf t
banner motd "---Attention---"
exit
copy run start
```
**Настроить время :**
```
clock set 22:00:00 22 oct 2024
copy run start
```

#### Часть 2. Создание сетей VLAN и назначение портов коммутатора

* Шаг 1. Создайте сети VLAN на коммутаторах:

**S1 И S2 :**
```
enable
configure terminal
vlan 10
 name Management
vlan 20
 name Sales
vlan 30
 name Operations
vlan 999
 name Parking_Lot
vlan 1000
 name Own
```
**S1 настройка ip для Vlan 10:**
```
interface vlan 10
ip address 192.168.10.11 255.255.255.0
ip default-gateway 192.168.10.1
no shutdown
exit
copy run start
``` 
**S2 настройка ip для Vlan 10:**
```
interface vlan 10
ip address 192.168.10.12 255.255.255.0
ip default-gateway 192.168.10.1
no shutdown
copy run start
```
c.	Назначьте все неиспользуемые порты коммутаторов S1 и S2 VLAN Parking_Lot,настройте их для статического режима доступа и административно деактивируйте их<br/>
**S1:**
```
interface range f0/2-4, f0/7-24, g0/1-2
sitchport mode access
switchport access vlan 999
shutdown
```
```
interface f0/6
switchport mode accesss
switchport access vlan 20
```
**S2:**
```
interface range f0/2-17, f0/19-24, f0/1-2
switchport mode access
switchport access vlan 999
shutdown
```
```
interface f0/18
switchport mode access
switchport access vlan 30
```
**![show vlan:](scrn/show%20vlan.png)**

#### Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

**Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторах S1 и S2.**

```
interface f0/1
switchport mode trunk
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
exit
copy run start
```
![show interface trunk](scrn/F01.png)

Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

```
interface f0/1
switchport mode trunk
switchport native vlan 1000
switchport trunk allowed vlan 10,20,30,1000
exit
copy run start
```
show interface f0/5 switchport<br/>
![show interface f0/5 switchport](scrn/F05.png)

#### Часть 4. Настройка маршрутизации между сетями VLAN

Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.
```
interface g0/0/1.10
encapsulation dot1q 10
ip address 192.168.10.1 255.255.255.0
description Management vlan
```
```
interface G0/0/1.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
description Sales VLAN
```
```
interface G0/0/1.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
description Operations VLAN
```
```
interface G0/0/1.1000
encapsulation dot1Q 1000 native

```
![R1](scrn/VlanR1.png)

#### Часть 5. Проверьте, работает ли маршрутизация между VLAN

a.	Отправьте эхо-запрос с PC-A на шлюз по умолчанию:

![ping PC-a](scrn/pingPC-Agateway.png)

b.	Отправьте эхо-запрос с PC-A на PC-B.

![ping PC-AB](scrn/ping%20PC-AB.png)

c.	Отправьте команду ping с компьютера PC-A на коммутатор S2.

![ping S2](scrn/pingS2.png)

В окне командной строки на PC-B выполните команду tracert на адрес PC-A:

![tracert](scrn/tracertPC-A.png)
