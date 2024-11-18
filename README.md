# ZFS
# Описание домашнего задания


1. Определить алгоритм с наилучшим сжатием:
- Определить какие алгоритмы сжатия поддерживает zfs (gzip, zle, lzjb, lz4);
- создать 4 файловых системы на каждой применить свой алгоритм сжатия;
- для сжатия использовать либо текстовый файл, либо группу файлов.
2. Определить настройки пула.
С помощью команды zfs import собрать pool ZFS.
Командами zfs определить настройки:
- размер хранилища;
- тип pool;
- значение recordsize;
- какое сжатие используется;
- какая контрольная сумма используется.
3. Работа со снапшотами:
- скопировать файл из удаленной директории;
- восстановить файл локально. zfs receive;
- найти зашифрованное сообщение в файле secret_message.


Дано:

![image](https://github.com/user-attachments/assets/2e78fc59-1dbb-422d-aad8-3e88126fde1c)


# 1. Определить алгоритм с наилучшим сжатием:

- Определить какие алгоритмы сжатия поддерживает zfs (gzip, zle, lzjb, lz4);
- создать 4 файловых системы на каждой применить свой алгоритм сжатия;
- для сжатия использовать либо текстовый файл, либо группу файлов.

Создаём пулы из двух дисков в режиме RAID1: 

zpool create otus1 mirror /dev/vdb /dev/vdc

zpool create otus2 mirror /dev/vdd /dev/vde

zpool create otus3 mirror /dev/vdf /dev/vdg

zpool create otus4 mirror /dev/vdh /dev/vdi


zpool list

![image](https://github.com/user-attachments/assets/43a09b6f-b7b3-404e-8542-e205ef868e79)


zfs set compression=lzjb otus1    #Алгоритм lzjb

zfs set compression=lz4 otus2     #Алгоритм lz4

zfs set compression=gzip-9 otus3  #Алгоритм gzip

zfs set compression=zle otus4     #Алгоритм zle


Выкачиваем файл с задания во все пуллы:

do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done
и смотрим результат:

![image](https://github.com/user-attachments/assets/1d1a143f-e6be-43c9-b0c1-9fff63088d41)


![image](https://github.com/user-attachments/assets/ce42c49f-c322-48ce-bc7a-87efeef1d355)

zip-9 самый эффективный по сжатию

# 2. Определить настройки пула.

С помощью команды zfs import собрать pool ZFS.
Командами zfs определить настройки:
- размер хранилища;
- тип pool;
- значение recordsize;
- какое сжатие используется;
- какая контрольная сумма используется.

качаем архив с задания:

wget -O archive.tar.gz --no-check-certificate 'https://drive.usercontent.google.com/download?id=1MvrcEp-WgAQe57aDEzxSRalPAwbNN1Bb&export=download'

разархивируем:

tar -xzvf archive.tar.gz

Проверка возможности импорта:

zpool import -d zpoolexport/

![image](https://github.com/user-attachments/assets/b367f636-706f-4939-bc17-7306babfdfb6)


zpool import -d zpoolexport/ otus

zpool status

![image](https://github.com/user-attachments/assets/5164efc0-b3e0-4570-a5b6-ee207b19f1b7)

zfs get all otus    #Определение всех параметров ФС, где вместо all можно указать любой из перечисленных парметров.


- размер хранилища;
  
![image](https://github.com/user-attachments/assets/ba46dec3-7b32-4d51-8428-16132d508053)

- тип pool;
  
![image](https://github.com/user-attachments/assets/e9a2b2ae-59a2-4ed3-8c86-d432ae2fec4c)

- значение recordsize;

![image](https://github.com/user-attachments/assets/381ffc57-5380-4a89-b743-43d0630339cb)

- какое сжатие используется;

![image](https://github.com/user-attachments/assets/011f153b-c1fe-4963-b357-336009d63dac)

- какая контрольная сумма используется.
  
![image](https://github.com/user-attachments/assets/aaeeb065-993d-431e-9f4e-40f92e77f8dc)

# 3. Работа со снапшотами:

- скопировать файл из удаленной директории;
- восстановить файл локально. zfs receive;
- найти зашифрованное сообщение в файле secret_message.

Качаем файл с задания:

wget -O otus_task2.file --no-check-certificate https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI&export=download

zfs receive otus/test@today < otus_task2.file

cat /otus/test/task1/file_mess/secret_message

![image](https://github.com/user-attachments/assets/8657f6a1-4e3e-45ba-91da-8d9d7a71e584)



https://otus.ru/lessons/linux-hl/
