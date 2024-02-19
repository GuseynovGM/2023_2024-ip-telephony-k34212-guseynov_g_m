University: ITMO University

Faculty: FICT

Course: IP-telephony

Year: 2023/2024

Group: K34212

Author: Guseynov Guseyn Muradovich

Lab: Lab1

Date of create: 19.02.2024

Date of finished: 


## Отчет по лабораторной работе №1:
### "Базовая настройка ip-телефонов в среде Сisco packet tracer"

#### 1. Цель:
Изучить рабочую среду Cisco Packet Tracer, ознакомиться с интерфейсами основных устройств, типами кабелей, научиться собирать топологию. Изучить построение сети IP-телефонии с помощью маршрутизатора, коммутатора и IP телефонов Cisco 7960 в среде Packet tracer.

#### 2. Ход работы:

**Часть 1.**

На основе первой схемы создана модель сети в которой расположено 7 ПК и 4 коммутатора. 

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/1.png)

Каждому ПК был присвоен IP адрес из диапазона от 192.168.0.1/24 до 192.168.0.7/24 в соответствии с номером PC

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/2.png)

Проведена проверка на корректность работы сети путем проведения команды ping с PC5 до PCО  

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/3.png)

Команда ping подтвердила успешное соединение компьютеров внутри сети

**Часть 2.**

На основе второй схемы создана модель сети в которой расположено 2 IP телефона, 1 коммутатор и 1 маршрутизатор. 

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/4.png)

На маршрутизаторе происвоим IP–адрес 192.168.0.1/24:

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/5.png)

Также на маршрутизаторе настроим  DHCP-сервер, с выделенным пулом для IP–телефонов:

```
Router(config)#ip dhcp pool voice
Router(dhcp-config)#network 192.168.0.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.0.1
Router(dhcp-config)#option 150 ip 192.168.0.1
```

Теперь настроим на маршрутизаторе VoIP параметры, а именно максимально возможное количество поддерживаемых номеров и телефонов, присвоение линий в автоматическом режиме:

```
Router(config)#telephony-service
Router(config-telephony)#max-dn 10
Router(config-telephony)#max-ephones 10
Router(config-telephony)#ip source-address 192.168.0.1 port 3100
Router(config-telephony)#auto assign 1 to 19
```

На коммутаторе включим поддержку VoIP для интерфейсов с подключенными IP телефонами (голосовой VLAN):

```
Switch(config)#int fa 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface range FastEthernet 0/23-24
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport voice vlan 1
```

Присвоим телефонам номера на маршрутизаторе:

```
Router(config)#ephone-dn 1
Router(config-ephone-dn)#number 10001
Router(config-ephone-dn)#ex
Router(config)#ephone-dn 2
Router(config-ephone-dn)#number 10010
```

Проверим работу устройств позвонив с IP телефона 0 на IP телефон 1 по созданному номеру

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab1/images/6.png)

Видим, что телефоны успешно соединены

## Вывод:

По итогу выполнения лабораторной работы был изучен базовый функционал Cisco Packet Tracer, были построены две сети и была произведена их настройка, а также изучена настройка IP-телефонии, на основе второй сети.
