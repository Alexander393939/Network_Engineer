# Лабораторная работа. Базовая настройка коммутатора

#### Топология:
![Топология](Топология.png) 

![Таблица адресации](Таблица%20адресации.png)

###	Задачи:
[_Часть 1._ Проверка конфигурации коммутатора по умолчанию](#section1)

_Часть 2._ Создание сети и настройка основных параметров устройства<br/>
[•	Настройте базовые параметры коммутатора.<br/>](#section2)
[•	Настройте IP-адрес для ПК.](#section3)

_Часть 3._ Проверка сетевых подключений<br/>
[•	Отобразите конфигурацию устройства.<br/>](#section4)
•	Протестируйте сквозное соединение, отправив эхо-запрос.<br/>
•	Протестируйте возможности удаленного управления с помощью Telnet.

### Решение:

## Часть 1

<h4 id="section1">Проверка конфигурации коммутатора по умолчанию:<h4/>

###### _Просмотр текущей конфигурации:_

```
show running-config
```

[вывод команды](config/)

##### При использовании Netlab отключите интерфейс F0/6 на коммутаторе S1

###### _Режим глобальной конфигурации:_
```
configure terminal
```
###### _Режим конфигурации интерфейса FastEthernet 0/6:_
```
interface FastEthernet 0/6
```
###### _Отключения интерфейса_
```
shutdown
```
###### _Сохранить изменения_
```
Switch(config-if)# exit
Switch(config)# exit
Switch# write memory
```

###### _Проверить состояние интерфейсов_
```
show ip interface brief
```
[вывод команды](config/)

###### _ограничить с помощью пароля доступ к привилегированному режиму_
```
enable secret <пароль>
```

## Часть 2

<h4 id="section2">Настройка базовых параметров коммутатора:<h4/>

a.	В режиме глобальной конфигурации скопируйте следующие базовые параметры конфигурации и вставьте их в файл на коммутаторе S1.<br/>
```
no ip domain-lookup

hostname S1

service password-encryption

enable secret class

banner motd #
Unauthorized access is strictly prohibited. #
```
a. [базовые параметры](config/)

b.	Назначьте IP-адрес интерфейсу SVI на коммутаторе. 

Процесс настройки IP-адреса:
```
S1> enable
S1# configure terminal
S1(config)# hostname S1
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown
S1(config-if)# exit
S1(config)# exit
S1# write memory

```

d.	Настройте каналы виртуального соединения для удаленного управления (vty), чтобы коммутатор разрешил доступ через Telnet.

_Процесс настройки_:

```
S1> enable
S1# configure terminal
S1(config)# line vty 0 
S1(config-line)# transport input telnet
S1(config-line)# password cisco
S1(config-line)# login
S1(config-line)# exit
S1(config)# exit
S1# write memory
```
###### Команда login используется в VTY,она указывает,что для доступа к коммутатору через Telnet или SSH необходимо ввести пароль.


 <h4 id="section3">2. Настройте IP-адрес на компьютере PC-A.<h4/>

 [ip-адрес PC-A](ip-адрес%20PC-A.png)

 ## Часть 3

 <h4 id="section4">Отобразите конфигурацию коммутатора.<h4/>

 [config](config/config)