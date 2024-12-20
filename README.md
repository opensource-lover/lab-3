# Лабораторная работа 3
Для решения был выбран гипервизор QEMU-KVM, поскольку он уже был установлен. 

![image](https://github.com/user-attachments/assets/7a166c2e-3c8d-4ffc-a29b-8c944b21ae94)

## Создание трех виртуальных машин 
В качестве ОС я выбрал Manjaro XFCE minimal, потому что у меня уже был образ, и установка происходит быстро. 

![image](https://github.com/user-attachments/assets/31b8f428-4297-4cc8-a23e-3e102084f442)

## Обеспечение доступа в сеть Интернет
Настройки по-умолчанию обеспечивают доступ в интернет на каждой из виртуальных машин. Для виртуальной локальной сети создаётся виртуальный свитч (virbr = virtual brigde = virtual switch), связывающий устройства, IP-адресса которых попадают в диапазон адресов 192.168.122.2 - 192.168.122.254 (у самого свитча адрес 192.168.122.1). После свитча пакеты попадают в виртуальный роутер, который и обеспечивает соединение с интернетом.

![image](https://github.com/user-attachments/assets/2fde2191-2ad8-4289-bb6f-06d834f295ac)

## Обеспечение доступа из A в B и С
Поскольку все три виртуальные машины принадлежат одной виртуальной локальной сети, требуемый доступ так же есть по-умолчанию, проверить его можно с помощью команды ping:

![image](https://github.com/user-attachments/assets/15cf39e9-fb90-4e67-b92b-b7ff2829a5cd)

## Запрет доступа из B в C

### Неудачная попытка

Для решение этой задачи возникла идея создать три виртуальных локальных сети, DHCP диапазоны двух из которых не будут пересекаться. 

![image](https://github.com/user-attachments/assets/f2d3c801-b732-4d55-b858-cec8076103ca)

Что не дало результата по ставшим понятными впоследствии причинам: 

![image](https://github.com/user-attachments/assets/3cfda3c9-bd58-4893-b6fc-d249b8d5761f)

### Удачная попытка

Обращаюсь к источнику, в котором есть всё, нахожу [гайд невадского университета по аналогичной задаче](https://csint.unr.edu/downloads/lesson-pdfs/08_Networking-RET.pdf), который указывает на возможность добавления второй виртуальной сетевой карты, что я и делаю для машины А:

![image](https://github.com/user-attachments/assets/00baa465-7dea-4107-be32-2b091787db98)

Новый план состоит в следующем: сделать по одной сети для B и для C, а А подключить к обоим сетям с помощью второго созданного NIC:

![image](https://github.com/user-attachments/assets/037f74e9-f2d9-405a-9a58-07431b95f1a3)

![image](https://github.com/user-attachments/assets/443a1f76-9711-4cd1-80ab-3f869277bd2f)

Схематичное изображение:

![image](https://github.com/user-attachments/assets/65e4f90b-c1ab-49d6-aec5-8cc2d57a6b7d)

Итоговая конфигурация карт:

![image](https://github.com/user-attachments/assets/5691536e-ef43-4593-9464-f76ba284520d)

Доступ к интернету по-прежнему есть у каждой из трех машин:

![image](https://github.com/user-attachments/assets/f03dec0c-3466-4799-9c63-d1ec4934bc52)

А вот пингануть C из B и B из C не удаётся:
![image](https://github.com/user-attachments/assets/4e66dd08-f0a4-4794-a6ec-5330deff925e)

Что и было целью. 
