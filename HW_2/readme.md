# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

## Задачи:
 ### 1. Создание и настройка сети
 ### 2. Изучение таблицы МАС-адресов коммутатора
     
## Топология:
![0](https://github.com/user-attachments/assets/11a8ddb0-52a3-4492-bb6b-8ef3a3e9699d)


## Таблица адресации:

| Устройство    | Интерфейс          | IP-адрес / префикс |
| ------------- |:------------------:| ------------------:|
| S1            |VLAN 1              | 192.168.1.11/24    |
| S2            |VLAN 1              | 192.168.1.12/24    |
| PC0           |NIC                 | 192.168.1.1/24     |
| PC1           |NIC                 | 192.168.1.2/24     |

## Решение:

## ЗАДАЧА 1. Создание и настройка сети

### Шаг 1. Подключаем сеть в соответствии с топологией:

![1](https://github.com/user-attachments/assets/12de572d-d889-44d7-8d58-df3b9a4f0dd8)

### Шаг 2. Настраиваем узлы ПК:

![2](https://github.com/user-attachments/assets/abcb533e-421f-4f0f-94fd-97168d7c11e9)

![3](https://github.com/user-attachments/assets/22e5c40e-a992-4665-987e-d4409a6d4f51)

### Шаг 3. Выполняем инициализацию и перезагрузку коммутаторов:

-Подключаем консольный кабеоль к PC0 и S1

![6](https://github.com/user-attachments/assets/45c5c72b-2aba-4a78-ad71-d294a4209be6)

### Шаг 4. Настраиваем базовые параметры каждого коммутатора:

-Настроены имена устройств в соответствии с топологией 

-Настроены IP-адреса, как указано в таблице адресации:

![4](https://github.com/user-attachments/assets/fc05517f-9d4f-463d-bbc0-543473388499)

![5](https://github.com/user-attachments/assets/018f41b2-614c-41ea-8c30-298c68c15022)

-Назначаем cisco в качестве паролей консоли и VTY

-Назначаем cisco в качестве пароля доступа к привилегированному режиму EXEC

![7](https://github.com/user-attachments/assets/82819efd-d1ba-4084-b3d1-2eb7e9e86e55)

![8](https://github.com/user-attachments/assets/5101e3b9-e386-4b2c-8a37-08b2462344cb)


## ЗАДАЧА 2. Изучение таблицы МАС-адресов коммутатора

### Шаг 1. Записываем МАС-адреса сетевых устройств:

-Открываем командную строку на PC0 и PC1 и вводим команду ipconfig /all:

![9](https://github.com/user-attachments/assets/32f82cc4-1978-4b01-86bb-ed5e2770c60a)

-Физические адреса адаптера Ethernet:
 
 MAC-адрес компьютера PC0: 00D0.58C8.05B2
 
 MAC-адрес компьютера PC1: 0003.E467.E8E6

-Подключаемся к коммутаторам S1 и S2 через консоль и вводим команду show interface F0/1 на каждом коммутаторе

МАС-адрес коммутатора S1 Fast Ethernet 0/1: 0006.2ae9.3301

![10](https://github.com/user-attachments/assets/b1019310-724a-45ee-a5f8-36cebb7ea5db)

МАС-адрес коммутатора S2 Fast Ethernet 0/1: 00d0.d33b.b5d8

![11](https://github.com/user-attachments/assets/b96ec74b-0aa8-4d63-8d2c-46365856c1d3)

### Шаг 2. Смотрим таблицу МАС-адресов коммутатора:

-Подключаемся к коммутатору S2 через консоль PC0 и входим в привилегированный режим EXEC:

![12](https://github.com/user-attachments/assets/77830484-1bd3-476a-bddc-d04408f6ab39)

-В привилегированном режиме EXEC вводим команду show mac address-table:

![13](https://github.com/user-attachments/assets/dbe78212-4298-43fc-bf50-9c6fd3f7db81)

В таблице MAC-адресов содержаться два адресса: коммутатора S1 , PC0 и PC1

### Шаг 3. Очищаем таблицу МАС-адресов коммутатора S2 и снова отображаем таблицу МАС-адресов:

-В привилегированном режиме EXEC вводжим команду clear mac address-table dynamic:

![14](https://github.com/user-attachments/assets/fa7683db-d132-4e0e-bc4f-ce8a5b74c77f)

-Снова быстро вводим команду show mac address-table:

![15](https://github.com/user-attachments/assets/d4614884-38c7-4d7a-8f15-7b0c380d021b)

В таблице отсутствует адресс РС1

-Через 10 секунд вводим команду show mac address-table:

В таблице отсутствует адресс РС1

![16](https://github.com/user-attachments/assets/75cdf782-50b7-4899-8b46-84e9d47dae97)

### Шаг 4. С компьютера PC1 отправляем эхо-запросы устройствам в сети и смотрим таблицу МАС-адресов коммутатора:

-На компьютере PC1 открываем командную строку и вводим команду arp -a

![17](https://github.com/user-attachments/assets/a5f457bf-b192-4e90-b7f3-bd494f567b1c)

В кэше ARP может не быть записей, или он может иметь сопоставление IP-адреса шлюза с MAC-адресом

-Из командной строки PC1 отправляем эхо-запросы на компьютер PC0, а также коммутаторы S1 и S2:

![18](https://github.com/user-attachments/assets/b6409e9c-0d79-4856-b60d-9366a44ac447)

![19](https://github.com/user-attachments/assets/abbfde40-b59e-4233-ac5e-c53ed4e9fe82)

![20](https://github.com/user-attachments/assets/79576e7c-8a35-4957-86e2-b11fb430543b)

-Подключившись через консоль к коммутатору S2, вводим команду show mac address-table

![21](https://github.com/user-attachments/assets/2fe73df1-f999-49db-88a1-0c369a8ce9bc)

-На компьютере PC1 открываем командную строку и еще раз вводим команду arp -a:

![22](https://github.com/user-attachments/assets/fefed0aa-e529-441d-b848-9b263b824f2d)

