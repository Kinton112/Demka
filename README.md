Краткое описание вашего проекта.

## Статус проекта

[![Статус](https://img.shields.io/badge/статус-активный-brightgreen)](ссылка_на_статус)

## Содержание

- [Базовая_Настройка](#Базовая_Настройка)
- [Установка](#установка)
- [Использование](#использование)
- [Вклад](#вклад)
- [Лицензия](#лицензия)
- [Контакты](#контакты)

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

## Установка



