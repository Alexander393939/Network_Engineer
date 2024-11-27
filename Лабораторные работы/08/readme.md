# Лабораторная работа - Настройка DHCPv6

#### Топология :
![Топология](scrn/Топология.png)

#### Таблица адресации :

|Устройство|Интерфейс|Ipv6-адрес|
|:-------:|:----------:|:-----------:|
|R1| G0/0/0|2001:db8:acad:2::1/64<br/>fe80::1|
|R1| G0/0/1|2001:db8:acad:1::1/64<br/>fe80::1|
|R2| G0/0/0|2001:db8:acad:2::2/64<br/>fe80::2|
|R2| G0/0/1|2001:db8:acad:3::1/64<br/>fe80::1|
|PC-A|NIC|DHCP|
|PC-B|NIC|DHCP|

 __Задачи__:<br/>
- Часть 1. Создание сети и настройка основных параметров устройства<br/>
- Часть 2. Проверка назначения адреса SLAAC от R1<br/>
- Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1<br/>
- Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1<br/>
- Часть 5. Настройка и проверка DHCPv6 Relay на R2<br/>

##### Часть 1:

4. Настройка интерфейсов и маршрутизации для обоих маршрутизаторов:

- a) Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.
 
- **Настройка R1 :** 
```
interface g0/0/0
ipv6 address 2001:db8:acad:2::1/64
ipv6 address fe80::1 link-local
no shutdown
exit

interface g0/0/1 
ipv6 address 2001:db8:acad:1::1/64
ipv6 address fe80::1 link-local
no shutdown
exit

```
- b) Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес G0/0/0 на другом маршрутизаторе

```
ipv6 route ::/0 2001:db8:acad:2::2
copy run start
```
![R1](scrn/R1showipv6.png)

- **Настройка R2 :**
```
interface g0/0/0
ipv6 address 2001:db8:acad:2::2/64
ipv6 address fe80::2 link-local
no shutdown
exit

interface g0/0/1
ipv6 address 2001:db8:acad:3/64
ipv6 address fe80::1 link-local
no shutdown
exit

ipv6 route ::/0 2001:db8:acad2::1
copy run start
```
![R2](scrn/R2showipv6.png)

#### c)	Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1:

![pingR1](scrn/PingR2изR1.png)


### Часть 2. Проверка назначения адреса SLAAC от R1

![PC-A](scrn/PC-A_ipconfig.png)

- Откуда взялась часть адреса с идентификатором хоста?

Идентификатор хоста 2E0:B0FF:FE93:C574 получился из мас-адреса 00E0.B093.C574

-мас-адрес 00E0.B093.C574 делиться на две части по 24 бита и между этими частями добавляется
 FFFE 

- 00E0BO**FFFE**93C574

-Седьмой бит адреса меняется на противоположный (1-на 0, 0- на еденицу)

- первый байт 00=000000**0**0- меняем 7 бит на **1** получится 00000010=02 **2E0:B0FF:FE93:C574**

### Часть 3. Настройка и проверка сервера DHCPv6 на R1

```
ipv6 dhcp pool R1-STATELESS
dns-server 2001:db8:acad::254
domain-name STATELESS.com
exit

interface g0/0/1
ipv6 nd other-config-flag
ipv6 dhcp server R1-STATELESS 
```
![PC-aDHCPv6R1](scrn/DHCPv6R1.png)

- f)	Тестирование подключения с помощью пинга IP-адреса интерфейса G0/1 R2

![pingR2](scrn/pingPC-A_R2.png)

### Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1:

 - a.	Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.

```
ipv6 dhcp pool R2-STATEFUL
address prefix 2001:db8:acad:3:aaa::/80
dns-server 2001:db8:acad::254
domain-name STATEFUL.com
exit

interface g0/0/0
ipv6 dhcp server R2-STATEFUL
```

### Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.
 настроить и проверить ретрансляцию DHCPv6 на R2, позволяя PC-B получать адрес IPv6.

 1. Включите PC-B и проверьте адрес SLAAC, который он генерирует.

 ![Pc-B](scrn/PC-B-slaac.png)

 2. Настройте R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1.

 ```
 interface g0/0/1
 ipv6 nd managed-config-flag
 ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
 exit
 copy run start
 ```
 