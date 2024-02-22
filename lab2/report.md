University: ITMO University

Faculty: FICT

Course: IP-telephony

Year: 2023/2024

Group: K34212

Author: Guseynov Guseyn Muradovich

Lab: Lab2

Date of create: 22.02.2024

Date of finished: 

## Отчет по лабораторной работе №2:
### "Конфигурация voip в среде Сisco packet tracer"

#### 1. Цель:
Изучить построение сети IP-телефонии с помощью маршрутизатора Cisco 2811, коммутатора Cisco catalyst 3560 и IP телефонов Cisco 7960.

#### 2. Ход работы:

**Часть 1.**

На основе схемы создана модель сети в которой расположено 3 IP-телефона, 1 коммутатор и 1 маршрутизатор. 

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/1.png)

Сменим наименование маршрутизатора и пропишем ему IP адрес 192.168.0.1/24.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/2.png)

Отключим на маршрутизаторе синтаксис ввода слов от DNS серверов:

```
CMERouter(config)#no ip domain-lookup
```

И зададим пароли для защиты маршрутизатора в удаленном режиме и в режиме консоли:

```
CMERouter(config)#line vty 0 4
CMERouter(config-line)#password password123
CMERouter(config-line)#login
CMERouter(config-line)#exit
CMERouter(config)#line console 0
CMERouter(config-line)#password password123
CMERouter(config-line)#login
```

Потом настроим на маршрутизаторе DHCP-сервер, выделив для IP–телефонов пул номеров:

```
CMERouter(config)#ip dhcp pool voice
CMERouter(dhcp-config)#network 192.168.0.0 255.255.255.0
CMERouter(dhcp-config)#default-router 192.168.0.1
CMERouter(dhcp-config)#option 150 ip 192.168.0.1
```

Теперь настроим на маршрутизаторе VoIP параметры, а именно максимально возможное количество поддерживаемых номеров и телефонов, присвоение линий в автоматическом режиме:

```
Router(config)#telephony-service
Router(config-telephony)#max-dn 10
Router(config-telephony)#max-ephones 10
Router(config-telephony)#ip source-address 192.168.0.1 port 3100
Router(config-telephony)#auto assign 1 to 10
```

На коммутаторе включим поддержку VoIP для интерфейсов с подключенными IP телефонами (голосовой VLAN):

```
Switch(config)#int fa 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport voice vlan 1
Switch(config-if)#interface range FastEthernet 0/22-24
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport voice vlan 1
```

Присвоим телефонам номера на маршрутизаторе:

```
Router(config)#ephone-dn 1
Router(config-ephone-dn)#number 10001
Router(config-ephone-dn)#ex
Router(config)#ephone-dn 2
Router(config-ephone-dn)#number 10002
Router(config-ephone-dn)#ex
Router(config)#ephone-dn 3
Router(config-ephone-dn)#number 10003
Router(config-ephone-dn)#ex
```

Проверим работу устройств позвонив по созданному номеру с IP телефона 0 на IP телефон 1 и с IP телефона 0 на IP телефон 2.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/3.png)

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/4.png)

Видим, что телефоны успешно соединены

**Часть 2.**

На основе второй схемы создана модель сети в которой расположено 3 IP телефона, 3 ПК, 1 коммутатор и 1 маршрутизатор.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/5.png)

На маршрутизаторе происвоим IP–адрес 192.168.0.1/24 и пропишим Vlan 10 и 20.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/6.png)

Пропишим указанные vlan'ы на порт fa0/1.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/7.png)

Переведем интерфейсы коммутатора, подключенные к ip-телефонам, в режим access для vlan 10 и 20 в качестве голосового:

```
Switch(config)#int range FastEthernet 0/22-24
Switch(config-if-range)#switchport access vlan 10
Switch(config-if-range)#switchport voice vlan 20
```

 Поднимем на маршрутизаторе саб-интерфейсы, соответсвующие vlan и назначим им IP–адреса:

 ```
Router(config)#int fa0/0
Router(config-if)#no shutdown
Router(config-if)#int fa0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.10.1 255.255.255.0
Router(config-subif)#int fa0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.20.1 255.255.255.0
```

Также на маршрутизаторе настроим  DHCP-сервер, с выделенным пулом для IP–телефонов:

```
Router(config)#ip dhcp pool data
Router(dhcp-config)#network 192.168.10.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.10.1
Router(dhcp-config)#ip dhcp pool voice
Router(dhcp-config)#network 192.168.20.0 255.255.255.0
Router(dhcp-config)#default-router 192.168.20.1
Router(dhcp-config)#option 150 ip 192.168.20.1
```

Проверим работу DHCP на ПК.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/8.png)

DHCP работает, теперь можем настроить VoIP параметры на маршрутизаторе:

```
Router(config)#telephony-service
Router(config-telephony)#max-dn 10
Router(config-telephony)#max-ephones 10
Router(config-telephony)#ip source-address 192.168.20.1 port 3100
Router(config-telephony)#auto assign 1 to 10
```

И перед проверкой работоспособности сети присвоим телефонам номера на маршрутизаторе:

```
Router(config)#ephone-dn 1
Router(config-ephone-dn)#number 10001
Router(config-ephone-dn)#ex
Router(config)#ephone-dn 2
Router(config-ephone-dn)#number 10002
Router(config-ephone-dn)#ex
Router(config)#ephone-dn 3
Router(config-ephone-dn)#number 10003
Router(config-ephone-dn)#ex
```

Проверим работу устройств позвонив по созданному номеру с IP телефона 2 на IP телефон 1 и с IP телефона 2 на IP телефон 0.

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/9.png)

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/10.png)

Видим, что телефоны успешно соединены и проверим корректность работы соеденения ПК в сети путем проведения команды ping с PC2 на PCО и PC1  

![](https://github.com/GuseynovGM/2023_2024-ip-telephony-k34212-guseynov_g_m/blob/main/lab2/images/11.png)

Команда ping подтвердила успешное соединение компьютеров внутри сети

## Вывод:

По итогу выполнения лабораторной работы были построены две сети и была произведена их настройка, а также подробно была изучена работа с маршрутизатором Cisco 2811, коммутатором Cisco catalyst 3560 и IP телефоном Cisco 7960. 
