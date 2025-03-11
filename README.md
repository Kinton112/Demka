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
Ещё есть строка `gateway 172.16.4.1` Пишем там где нужен gateway  
  
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

## HQ-RTR
Выполнить базовою настройку после идти ниже  
<b>vlan</b>  
```
apt install vlan
modprobe 8021q
echo 8021q >> /etc/modules  
```
Теперь переходим в настройку интерфейсов пример указан ниже:  




