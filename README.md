Краткое описание вашего проекта.

## Статус проекта

[![Статус](https://img.shields.io/badge/статус-активный-brightgreen)](ссылка_на_статус)

## Содержание

- [Базовая_Настройка](#Базовая_Настройка)
- [ISP](#ISP)
- [HQ-RTR](#HQ-RTR)
- [HQ-SRV](#HQ-SRV)
- [BR-RTR](#BR-RTR)
- [BR-SRV](#BR-SRV)
- [Доп_инф](#Доп_инф)

## Базовая_Настройка
<b>Настройка имени</b>  
Для начало нужно поменять именна на машинах   
`hostnamectl set-hostname Имя устройства`  

    
<b>Настройка портов</b>  
Далее нам нужно настроить порты на каждом устройстве пример настройки  
```
auto ens224  
iface ens224 inet static  
address 172.16.4.1  
netmask 255.255.255.240
```  
Ещё есть строка `gateway айпи` Пишем там где нужен gateway  
  
Дополнение:
  
1. ens*** - порт  
2. Айпишники и маски смотрим по заданию
  
<b>!!!ВНИМАНИЕ!!!</b>  
Если в задании не будут использоваться встроенные репозитории, а будет  
возможность скачивать все пакеты из интернета, необходимо отключить  
проверку пакетов через cdrom зайдя по пути:  
`Nano /etc/apt/sources.list`  

На HQ-RTR и BR-RTR Нужно зайти в файл `/etc/resolv.conf` и оставить там только одну строку: `nameserver 1.1.1.1`

Везде нужно зайти в файл `/etc/sysctl.conf` и раскомментировать строку `net.ipv4.ip_forward=1`

## ISP  
Выполнить базовою настройку после идти ниже  
<b>iptables</b>  
```
apt install iptables  
apt install iptables iptables-persistent  
iptables –t nat –A POSTROUTING –s 172.16.4.0/28 –o ens192 –j MASQUERADE  
iptables –t nat –A POSTROUTING –s 172.16.5.0/28 –o ens192 –j MASQUERADE  
iptables-save > /etc/iptables/rules.v4  
```
  
Айпишник это подсеть куда идёт nat, ens192 это порт с интернетом  

Далее проверяем вышло ли настроить:  
`iptables –L –t nat` 
<details>
<summary>✔️ Результат проверки</summary>

Здесь находится ваш текст, который будет скрыт до тех пор, пока пользователь не нажмет на заголовок.

</details>  

## HQ-RTR
Выполнить базовою настройку после идти ниже  
<b>vlan</b>  
```
apt install vlan
modprobe 8021q
echo 8021q >> /etc/modules  
```
Теперь переходим в настройку интерфейсов пример указан ниже:  
```
auto ens192  
iface ens192 inet static  
address 172.16.4.2  
netmask 255.255.255.240  
gateway 172.16.4.1  
  
auto ens224  
iface ens224 inet static  
address 192.168.100.1  
netmask 255.255.255.192  
  
auto ens224:1  
iface ens224 inet static  
address 192.168.200.1  
netmask 255.255.255.240  

auto ens224.100  
iface ens224 inet static  
address 192.168.100.3  
netmask 255.255.255.192  
Vlan-raw-device ens224  
  
auto ens224:1.200  
iface ens224 inet static  
address 192.168.200.3  
netmask 255.255.255.240  
Vlan-raw-device ens224:1
```
  
<b>iptables</b>  
`iptables –t nat –A POSTROUTING –s 192.168.100.0/26 –o ens192 –j MASQUERADE`  
  
<b>Создание пользователей</b>  
Создание 
`adduser Имя пользователя`  
  
Для смены id используется команда `usermode -u 1010 пользователь`  
Так же возможно понадобится выдать Root права для данных клиентов это можно выполнить 
посредством команды `visudo`
![user_root](user_root.png)


## Доп_инф
<details>
<summary>🎭 Таблица Масок</summary>

![Логотип проекта](Безымянный.png)

</details>  

