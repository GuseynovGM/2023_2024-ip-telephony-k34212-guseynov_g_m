University: ITMO University

Faculty: FICT

Course: IP-telephony

Year: 2023/2024

Group: K34212

Author: Guseynov Guseyn Muradovich

Lab: Lab2

Date of create: 26.02.2024

Date of finished: 

## Отчет по лабораторной работе №3:
### "Использование Asterisk в качестве SIP proxy"

#### 1. Цель:
Изучить программный комплекс Asterisk. Настройка Asterisk для локальных звонков.

#### 2. Ход работы:

**Часть 1.**

На виртуальную машину ubuntu установим Asterick

```
sudo apt install asterisk
```

Теперь мы отредактируем файл sip.conf и extensions.conf,  в каталоге /etc/asterisk, добавив в него информацию о телефонах 1000 и 1001. 

```
[1000]
type=friend
host=dynamic
secret=123
context=ext_1000

[1001]
type=friend
host=dynamic
secret=123
context=ext_1001
```

В файле sip.conf мы присвоили номерам тип “friend”, что позволяет им принимать и совершать вызовы, также разрешили им подключение с разных IP-адресов, указали пароль и имя контекстах. 

-----

```
[ext_1000]
exten => _XXXX,1,Dial(SIP/${EXTEN})

[ext_1001]
exten => _XXXX,1,Dial(SIP/${EXTEN})
```

В файле extensions.conf были созданы экстеншены для определенных ранее контекстов. Вызовы на них осуществляются по набранному номеру с использованием канала SIP.

После измененя файлов перезапустим Asterisk и проверим его статус

```
sudo service asterisk restart
```
```
sip show peer
```

Статус сервиса запущен, он работает.

**Часть 2.**

Теперь установим в качетсве soft телефона Zoiper5 и MicroSIP, оба устоновим с официальных сайтов. 

Настроим Zoiper5 через веб-инерфейс:

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab3/images/1.png)

Настроим MicroSIP через веб-инерфейс, предварительно устоновив wine:

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab3/images/2.png)

Проверим корректность настройки связи, мы совершили звонок с телефона 1001 на телефон 1000. 

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab3/images/3.png)

Соединение было успешно установлено, и на экране телефона 1000 отобрался номер телефон 1001.

## Вывод:

По итогу выполнения лабораторной работы была проведена настройка Asterisk для локальных звонков и настройка soft телефонов Zoiper5 и MicroSIP
