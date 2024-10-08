# Лабораторная работа 03 - Расчет подсетей IPv4

### Проблема1:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>192.168.200.139</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.255.255.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.255.224</td>
  </tr>
</table>

 Решение                                    |                                                                                 |
|---------------------------------------------|------------------------------------------------------------------------------------------|
| **Количество бит подсети**                  | Исходная маска подсети: 255.255.255.0 (/24) — 24 бита подсети.(**11111111.11111111.11111111**.00000000)<br> Новая маска подсети: 255.255.255.224 (/27) — **27 бит подсети** (**11111111.1111111.11111111.111**00000)|
| **Количество созданных подсетей**           |  Количество новых подсетей определяется количеством добавленных битов.<br> Добавлено 3 бита: 27 - 24 = 3.<br> Количество созданных подсетей: 2^3 = **8 подсетей**                          |
| **Количество бит узлов в подсети**          | 32 бита всего , значит 32-27=**5 бит**                                                   |
| **Количество узлов в подсети**              |Количество узлов=2^5 - 2 = **30**, минус 2 адреса: сетевой и широковещательный.                               |
| **Сетевой адрес этой подсети**              |192.168.200.139 маска 255.255.255.224<br>11000000.10101000.11001000.10001011<br>11111111.11111111.11111111.11000000<br>11000000.10101000.11001000.10000000<br>**Сетевой адрес- 192.168.200.128**|
| **IPv4-адрес первого узла в этой подсети**  |Первый адрес — это следующий за сетевым:<br> **192.168.200.129**    |
| **IPv4-адрес последнего узла в этой подсети**|Последний адрес — это адрес перед широковещательным:<br> **192.168.200.158**         |
| **Широковещательный IPv4-адрес в этой подсети**|Широковещательный адрес — нужно в сетевом адресе 192.168.200.128 установить все биты узловой части в 1<br>11000000.10101000.11001000.10011111<br> **Широковещательный адрес-192.168.200.159** 

### Проблема2:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>10.101.99.228</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.0.0.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.128.0</td>
  </tr>
</table>

| Решение                          |                                     |
|----------------------------------|-------------------------------------|
**Количество бит подсети** | **17 бит**|
**Количество созданных подсетей**| **512 подсетей**|
**Количество бит узлов в подсети** | **15 бит**
**Количество узлов в подсети** | **32766**
**Сетевой адрес этой подсети** | 10.101.99.228 маска 255.255.128.0 <br> 00001010.01100101.01100011.11100100<br>11111111.11111111.10000000.00000000<br>00001010.01100101.00000000.00000000<br>**10.101.0.0-Сетевой адрес**|
**IPv4-адрес первого узла в этой подсети** | **10.101.0.1**|
|**IPv4-адрес последнего узла в этой подсети**|   **10.101.127.254**              |
|**Широковещательный IPv4-адрес в этой подсети**| 10.101.0.0 установить биты узловой части в 1<br>00001010.01100101.01111111.11111111<br>**10.101.127.255**|

### Проблема3:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>172.22.32.12</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.255.0.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.224.0</td>
  </tr>
</table>

| Решение                          |                                     |
|----------------------------------|-------------------------------------|
**Количество бит подсети** | **19**|
**Количество созданных подсетей**| **8**|
**Количество бит узлов в подсети** | **13**
**Количество узлов в подсети** | **8190**
**Сетевой адрес этой подсети** | **172.22.32.0**|
**IPv4-адрес первого узла в этой подсети** | **172.22.32.1**|
|**IPv4-адрес последнего узла в этой подсети**|   **172.22.63.254**              |
|**Широковещательный IPv4-адрес в этой подсети**| **172.22.63.255**|

### Проблема4:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>192.168.1.245</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.255.255.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.255.252</td>
  </tr>
</table>

| Решение                          |                                     |
|----------------------------------|-------------------------------------|
**Количество бит подсети** | **30**|
**Количество созданных подсетей**| **64**|
**Количество бит узлов в подсети** | **2**
**Количество узлов в подсети** | **2**
**Сетевой адрес этой подсети** | **192.168.1.244**|
**IPv4-адрес первого узла в этой подсети** | **192.168.1.245**|
|**IPv4-адрес последнего узла в этой подсети**|   **192.168.1.246**              |
|**Широковещательный IPv4-адрес в этой подсети**| **192.168.1.247**|

### Проблема5:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>128.107.0.55</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.255.0.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.255.0</td>
  </tr>
</table>

 Решение                          |                                     |
|----------------------------------|-------------------------------------|
**Количество бит подсети** | **24**|
**Количество созданных подсетей**| **256**|
**Количество бит узлов в подсети** | **8**
**Количество узлов в подсети** | **254**
**Сетевой адрес этой подсети** | **128.107.0.0**|
**IPv4-адрес первого узла в этой подсети** | **128.107.0.1**|
|**IPv4-адрес последнего узла в этой подсети**|   **127.107.0.254**              |
|**Широковещательный IPv4-адрес в этой подсети**| **127.107.0.255**|

### Проблема6:)

<table style="width: 100%;">
  <tr>
    <th colspan="2" style="text-align:center;">Дано</th>
  </tr>
  <tr>
    <td><strong>IP-адрес узла</strong></td>
    <td>192.135.250.180</td>
  </tr>
  <tr>
    <td><strong>Исходная маска подсети</strong></td>
    <td>255.255.255.0</td>
  </tr>
  <tr>
    <td><strong>Новая маска подсети</strong></td>
    <td>255.255.255.248</td>
  </tr>
</table>

Решение                          |                                     |
|----------------------------------|-------------------------------------|
**Количество бит подсети** | **29**|
**Количество созданных подсетей**| **32**|
**Количество бит узлов в подсети** | **3**
**Количество узлов в подсети** | **6**
**Сетевой адрес этой подсети** | **192.135.250.176**|
**IPv4-адрес первого узла в этой подсети** | **192.135.250.177**|
|**IPv4-адрес последнего узла в этой подсети**|   **192.135.250.182**|
|**Широковещательный IPv4-адрес в этой подсети**| **192.135.250.183**|
