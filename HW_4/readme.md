# Лабораторная работа - Настройка IPv6-адресов на сетевых устройствах 

## Задачи:
### 1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
### 2. Ручная настройка IPv6-адресов
### 3. Назначнеие компьютерам статические IPv6-адреса
### 4. Проверка сквозного подключения


## Топология:
<img width="952" height="128" alt="изображение" src="https://github.com/user-attachments/assets/3a2cf7fd-8029-46d9-b996-21646efb1719" />

## Таблица адресации:

| Устройство    | Интерфейс          | IPv6-адрес         | Link local IPv6-адрес  | Длина префикса  | Шлюз по умолчанию |
| ------------- |:------------------:| :-----------------:| :---------------------:| :--------------:| -----------------:|
| R1            |G0/0/0              | 2001:db8:acad:a::1 |  fe80::1               | 64              | -                 |
| R1            |G0/0/1              | 2001:db8:acad:1::1 |  fe80::1               | 64              | -                 |
| S1            |VLAN 1              | 2001:db8:acad:1::b |  fe80::1               | 64              | -                 |
| PC1           |NIC                 | 2001:db8:acad:1::3 |  SLACC                 | 64              |fe80::1            |
| PC2           |NIC                 | 2001:db8:acad:a::3 |  SLACC                 | 64              |fe80::1            |


## Решение:

### Шаг 1. Создаем сеть согласно топологии

![Topology](https://github.com/user-attachments/assets/bb3a3e63-1cdb-43d4-a0c3-f3c9e6c9df82)

### Шаг 2. Настраиваем маршрутизатор (R1)
  Назначаем имя хоста и настраиваем основные параметры устройства, назначаем IPv6-адреса интерфейсам Ethernet на R1.

  ![R1](https://github.com/user-attachments/assets/970113c9-21b6-45c8-b42f-43edb2ee2bff)

  Вводим команду show ipv6 interface brief, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный IPv6-адрес:

  ![R2](https://github.com/user-attachments/assets/e0d8cdae-37c0-45b8-a0a2-214103d7adbd)

  Чтобы обеспечить соответствие локальных адресов канала индивидуальному адресу, вводим локальные адреса канала на каждом интерфейсе Ethernet на R1:

  ![R3_link](https://github.com/user-attachments/assets/1cc4c77f-7e9e-4a85-87af-d87754792bb6)

  Используем команду show ipv6 interface brief, чтобы убедиться, что локальный адрес связи изменен на fe80::1:

  ![R5](https://github.com/user-attachments/assets/ccbdc3bf-d87b-463f-b006-066bdb182927)

  --Какие группы многоадресной рассылки назначены интерфейсу G0/0?

    Joined group address(es):
    FF02::1
    FF02::1:FF00:1

  В командной строке на PC2 вводим команду ipconfig, чтобы получить данные IPv6-адреса, назначенного интерфейсу ПК.  

  ![ipconfig_PC2](https://github.com/user-attachments/assets/f1a1b6bb-e02a-4593-9089-77381aff7d19)

  Активируем IPv6-маршрутизацию на R1 с помощью команды IPv6 unicast-routing:

  ![R4](https://github.com/user-attachments/assets/e32eea65-d9ca-4251-b08e-a7062f538a90)

  После активации IPv6-маршрутизации на R1 с помощью команды IPv6 unicast-routing параметры адреса не изменяются (повторно вводим команду ipconfig на PC2):

  ![ipconfig_PC2](https://github.com/user-attachments/assets/f1a1b6bb-e02a-4593-9089-77381aff7d19)

  Введим команду show ipv6 interface g0/0/0, чтобы узнать, какие многоадресные группы присвоены интерфейсу G0/0/0:

  ![Безымянный](https://github.com/user-attachments/assets/f81e2154-aed2-445a-9296-67ecbf176bce)

### Шаг 3. Настраиваем коммутатор (S1)
    
Назначаем имя хоста и настраиваем основные параметры устройства, назначаем IPv6-адреса интерфейсам Ethernet на S1:

![S1](https://github.com/user-attachments/assets/c60ff5a7-9d52-42dd-9079-1f084eb4d793)

Установливаем шаблон dual-ipv4-and-ipv6 в качестве шаблона SDM по умолчанию, выполняем следующие действия:

S1# show sdm prefer

Затем:

![S1_SDM](https://github.com/user-attachments/assets/2d8952ea-d392-4024-a56f-26f9845f5d06)

 Активируем IPv6-маршрутизацию на S1 с помощью команды IPv6 unicast-routing:

 ![S1_ipv6](https://github.com/user-attachments/assets/03b27b29-4b76-44db-8eb9-b3743a1dfc16)

### Шаг 4. Назначнеие компьютерам статические IPv6-адрес

Назначаем Ipv6 адрес для PC1:

![config_pc1](https://github.com/user-attachments/assets/758101b7-24eb-41da-b7bb-816daa9e5d45)

Назначаем Ipv6 адрес для PC2:

![config_pc2](https://github.com/user-attachments/assets/5495c3b5-0cd8-4f73-95b8-6d71d2aa2aa8)

### Шаг 5.Проверка сквозного подключения

С PC1 отправляем эхо-запрос на FE80::1. Это локальный адрес канала, назначенный G0/1 на R1:

![PC1_ping_FE80](https://github.com/user-attachments/assets/2e13ba12-0583-47e3-8e5b-c309bf4a3255)

Отправляем эхо-запрос на интерфейс управления S1 с PC1:

![PC1_ping_S1](https://github.com/user-attachments/assets/d6c4548a-a628-434e-acbc-f2792a1d168b)

Вводим команду tracert на PC1, чтобы проверить наличие сквозного подключения к PC2:

![tracert_PC1_PC2](https://github.com/user-attachments/assets/b18e7836-6497-431d-954e-de5f18d54894)

С PC2 отправляем эхо-запрос на PC1:

![ping_pc2_pc1](https://github.com/user-attachments/assets/f7de0892-4168-48e8-b981-806ecde4ab4d)

С PC2 отправляем эхо-запрос на локальный адрес канала G0/0/0 на R1:

![ping_PC2_R1](https://github.com/user-attachments/assets/7ab8ef30-e0cd-40ca-9391-5af9312e429a)

## Вопросы для повторения:

1.	Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот же локальный адрес канала — FE80::1?
   
  Локальные адреса каналов (Link-Local) IPv6 (диапазон FE80::/10) уникальны только в пределах одного физического канала (сегмента сети или VLAN). 
  Поскольку интерфейсы маршрутизатора R1 подключены к разным сегментам, дублирование адреса \(FE80::1\) на них не вызывает конфликтов, так как 
  маршрутизатор использует их локально для каждого сегмента отдельно. 

2.	Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64?

  Адрес: 2001:db8:acad::aaaa:1234
  Развернутый вид: 2001:0db8:acad:0000:0000:0000:aaaa:1234
  Сеть (/64): 2001:0db8:acad:0000:: (первые 4 группы)
  Идентификатор подсети: Четвертая группа — 0000 (или 0). 







