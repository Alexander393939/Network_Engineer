# Лабораторная работа - Настройка протоколов CDP, LLDP и NTP

Топология 

![Топология](scrn/Топология.png)

Таблица адресации:

|Устройство|Интерфейс|Ip-адрес|Маска подсети|Шлюз по умолчанию|
|:--------------:|:-----------:|:----------:|:------------:|:-----------:|
|R1|Loopback1<br/>G0/0/1|172.16.1.1<br/>10.22.0.1|255.255.255.0<br/>255.255.255.0|------<br/>------|
|S1|SVI VLAN 1|10.22.0.2|255.255.255.0|10.22.0.1|
|S2|SVI VLAN 1|10.22.0.3|255.255.255.0|10.22.0.1|

Задачи<br/>
Часть 1. Создание сети и настройка основных параметров устройства<br/>
Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP<br/>
Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP<br/>
Часть 4. Настройка и проверка NTP<br/>

## Часть 1. Создание сети и настройка основных параметров устройства

Настройте базовые параметры для маршрутизатора.

```
enable
conf t
hostname R1
no ip domain-lookup
enable secret class

line console 0
password cisco
login

line vty 0 4
password cisco
login
exit

service password-encryption

banner motd "Alarm!"

interface loopback 1
ip address 172.16.1.1 255.255.255.0
exit

interface g0/0/1
ip address 10.22.0.1 255.255.255.0
no shutdown
end

copy run start

```
Настройте базовые параметры каждого коммутатора.

S1 и S2

```
enable 
conf t
hostname S1
no ip domain-lookup
enable secret class

line console 0
password cisco
login

line vty 0 4
password cisco
login
exit

service password-encryption

banner motd "Alarm!"
```
S1:
```
interface range f0/2-4, f0/6-24, g0/1-2
shutdown
end
copy run start

```
S2:
```
interface range f0/2-24, g0/1-2
shutdown
end
copy run start
```
