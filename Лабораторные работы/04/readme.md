# Лабораторная работа 04. Настройка ipv6-адресов на сетевых устройствах.

#### Топология:
![Топология](scrn/топология.png)

#### Таблица адресации:

| Устройство | Интерфейс    |IPv6-адрес    |Link local IPv6-адрес    | Длина префикса    | Шлюз по умолчанию     |
|:------------------:|:--------------:|:------:|:------:|:------:|:--------:|
R1 | G0/0/0 G0/0/1 | 2001:db8:acad:a::1 2001:db8:acad:1::1 |fe80::1<br> fe80::1 | 64<br>64| -| 
S1  |VLAN 1|2001:db8:acad:1::b|fe80::b|64|-|
PC-A|NIC|2001:db8:acad:1::3|SLACC|64|fe80::1|
PC-B|NIC|2001:db8:acad:a::3|SLACC|64|fe80::1|
 
 _<p style="margin-top: -20px;"><small style="font-size: 10px;">SLAAC (Stateless Address Autoconfiguration) в IPv6 — это механизм, который позволяет устройствам автоматически получать свой IPv6-адрес без использования DHCP-сервера.</small></p>_

#### **Часть 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора:**<br>  Шаг 1. Настройте маршрутизатор.

*  Настройте маршрутизатор. Назначьте имя хоста и настройте основные параметры устройства:

```
enable
conf t
hostname R1
service password-endcryption
enable secret cisco
line vty 0
transport input telnet
password class
login
end
copy run start
```
*  Настройте коммутатор. Назначьте имя хоста и настройте основные параметры устройства:
```
enable 
conf t
hostname S1
service password-endcryption
enable secret cisco
line vty 0
transport input telnet
password class
login
end
```
```
conf t
sdm prefer dual-ipv4-and-ipv6 default
end
reload
```
 _**sdm prefer dual-ipv4-and-ipv6 default** шаблон SDM, который поддерживает работу как с IPv4, так и с IPv6_

#### **Часть 2. Ручная настройка IPv6-адресов**
 a.	Назначьте глобальные индивидуальные IPv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1:
 ```
 conf t
 interface g 0/0/0
 ipv6 address 2001:db8:acad:a::1/64
 ipv6 address fe80::1 link-local
 exit
 interface g 0/0/1
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1 link-local
 end
 ```
 ```
 show ipv6 interface brief
 ```
 ![](srcn/R1ip.png)