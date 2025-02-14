# RAID

_____________________
RAID (англ. Redundant Array of Independent Disks — избыточный массив независимых (самостоятельных) дисков) — технология виртуализации данных для объединения нескольких физических дисковых устройств в логический модуль для повышения отказоустойчивости и (или) производительности.

RAID 5 — дисковый массив с чередованием блоков данных и контролем чётности

Основным недостатком уровней RAID от 2-го до 4-го является невозможность производить параллельные операции записи, так как для хранения информации о чётности используется отдельный контрольный диск. RAID 5 не имеет этого недостатка. Блоки данных и контрольные суммы циклически записываются на все диски массива, нет асимметричности конфигурации дисков. Под контрольными суммами подразумевается результат операции XOR (исключающее или). Xor обладает особенностью, которая даёт возможность заменить любой операнд результатом, и, применив алгоритм xor, получить в результате недостающий операнд. Например: a xor b = c (где a, b, c — три диска рейд-массива), в случае если a откажет, мы можем получить его, поставив на его место c и проведя xor между c и b: c xor b = a. Это применимо вне зависимости от количества операндов: a xor b xor c xor d = e. Если отказывает c, тогда e встаёт на его место и, проведя xor, в результате получаем c: a xor b xor e xor d = c. Этот метод по сути обеспечивает отказоустойчивость 5 версии. Для хранения результата xor требуется всего 1 диск, размер которого равен размеру любого другого диска в RAID.

Минимальное количество используемых дисков равно трём.

Достоинства:

RAID 5 получил широкое распространение, в первую очередь благодаря своей экономичности. Объём дискового массива RAID 5 рассчитывается по формуле (n−1)×S, где n — число дисков в массиве, а S — объём диска (наименьшего, если диски имеют разный размер). Например, для массива из четырёх дисков по 500 гигабайт общий объём будет (4−1)×500 = 1500 гигабайт, то есть «теряется» 25 % против 50 % RAID 10. С увеличением количества дисков в массиве экономия (по сравнению с другими уровнями RAID, обладающими отказоустойчивостью) продолжает увеличиваться. RAID 5 обеспечивает высокую скорость чтения — выигрыш достигается за счёт независимых потоков данных с нескольких дисков массива, которые могут обрабатываться параллельно.
Недостатки:

Производительность RAID 5 заметно ниже на операциях типа Random Write (записи в произвольном порядке), при которых производительность падает на 10—25 % от производительности RAID 0 (или RAID 10), так как требует большего количества операций с дисками (каждая операция записи, за исключением операций типа «full-stripe write», заменяется на контроллере RAID на четыре — две операции чтения и две операции записи).
При выходе из строя одного диска надёжность тома сразу снижается до уровня RAID 0 с соответствующим количеством дисков n−1, то есть в n−1 раз ниже надёжности одного диска — данное состояние называется критическим (degrade или critical). Для возвращения массива к нормальной работе требуется длительный процесс восстановления, связанный с ощутимой потерей производительности и повышенным риском. В ходе восстановления (rebuild или reconstruction) контроллер осуществляет длительное интенсивное чтение, которое может спровоцировать выход из строя ещё одного или нескольких дисков массива. Кроме того, в ходе чтения могут выявляться ранее не обнаруженные сбои чтения в массивах cold data (данных, к которым не обращаются при обычной работе массива, архивные и малоактивные данные), препятствующие восстановлению. Если до полного восстановления массива произойдет выход из строя, или возникнет невосстановимая ошибка чтения хотя бы на ещё одном диске, то массив разрушается и данные на нём восстановлению обычными методами не подлежат. Для предотвращения таких ситуаций в RAID-контроллерах может применяться анализ атрибутов S.M.A.R.T.

## **Пример настройки Linux**

Список дисков:
```
lsblk
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2e20c3f2-2ece-4229-832e-a288622b8ede)  

Создание:
```
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
watch cat /proc/mdstat
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/777719db-054f-4c0d-b444-94fc894ae1cc)  

```
mkfs.ext4 /dev/md0
mkdir /mnt/raid5
mount /dev/md0 /mnt/raid5
nano /etc/fstab
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c4fcf214-b4ef-467f-8523-808804b407d1)  

Проверяем работоспособность:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7875afe5-027e-46ca-aaa4-85ed688f0528)  


_____________________

Banner /etc/openssh/banner
PermitRootLogin no
PasswordAuthentication no
Port 2222
MaxAuthTries 4
PermitEmptyPasswords no
LoginGraceTime 5m
ClientAliveInterval 60
ClientAliveCountMax 5

______________________
### Модуль 3: Эксплуатация объектов сетевой инфраструктуры
## **Задание модуля 3:**

**1. Реализуйте мониторинг по средствам rsyslog на всех Linux хостах.**
**a. Составьте отчёт о том, как работает мониторинг**
https://sysahelper.ru/mod/book/view.php?id=253&chapterid=78


**2. Выполните настройку центра сертификации на базе HQ-SRV:**  
**a. Выдайте сертификаты для SSH;**  
**b. Выдайте сертификаты для веб серверов;**  

https://sysahelper.ru/mod/book/view.php?id=253&chapterid=79 

**3. Настройте SSH на всех Linux хостах:**  
**a. Banner ( Authorized access only! );**   
**b. Установите запрет на доступ root;**   
**c. Отключите аутентификацию по паролю;**   
**d. Переведите на нестандартный порт;**   
**e. Ограничьте ввод попыток до 4;**   
**f. Отключите пустые пароли;**   
**g. Установите предел времени аутентификации до 5 минут;**  
**h. Установите авторизацию по сертификату выданным HQ-SRV**   

Т.к. настройка всех параметров идентична для всех устройств пример будет показан на устройстве **HQ-SRV**. 
Все настройки заключаются в изменении конфигурационного файла **sshd_config**. Для удобства выполнения пользуйтесь поиском по файлу (ctrl+w)  
**a. Banner ( Authorized access only! );**  
```
nano /etc/openssh/banner
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/12507949-634f-4855-ba51-b974393d53e6)  
```
nano /etc/openssh/sshd_config
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7f8f99e3-dcd7-460a-b097-c11f55461a66)  
**b. Установите запрет на доступ root;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/bfafff53-62de-42a6-bd0f-61d6fc431ec1)  
**c. Отключите аутентификацию по паролю;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/884b7397-0c7b-40ff-800c-14ab8bfd77b7)  
**d. Переведите на нестандартный порт;**   
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/fa433de7-989c-4276-a5ef-bb9d70c0bb9b)  
**e. Ограничьте ввод попыток до 4;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9a941af5-e7c0-498f-ab40-b9dd304b6c0b)  
**f. Отключите пустые пароли;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/80a71064-3e7f-4b9b-a006-596d4d931449)  
**g. Установите предел времени аутентификации до 5 минут;**  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a0ec9ef0-8481-4602-b8ac-217e315c3b9a)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e311bd90-1427-456f-ae6f-835d3ba74348)  
**h. Установите авторизацию по сертификату выданным HQ-SRV**   
## расписать пункт

6. Настройте виртуальный принтер с помощью **CUPS** для возможности печати документов из Linux-системы на сервере BR-SRV.
Предположим, что печать должна осуществляться с устройства **CLI**
## **BR-SRV**
Открываем в браузере **http://localhost:631/admin** :
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2e5308fe-e651-4ff1-a6bb-15bbea4cbc33)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c4100fc6-0fad-45e4-8a63-72bbe4272163)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/540dce0c-8826-4085-8584-bcc181d5793e)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/095fa6bb-2e41-4c8d-bccc-92c110bcf0e8)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/13de7d6d-efdc-41ec-8ba8-fb14ee7f15d0)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/40c189bb-7d65-4d63-8058-e815604dbbd7)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/71e2ca26-5441-425d-8172-f7997eac6e0c)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/8741d74e-07c5-4342-b570-ef671f3c1fb7)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/4352d232-3669-4eea-b23f-dfbb623cf050)  

## **CLI**
Добавляем принтер:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/b439af24-a068-4dc8-a4af-184e13cf51ce)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/e6ddb986-f59a-4e65-8668-c4252f0c0604)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/9c9dd491-a171-46d3-9098-68cd43e356e7)  
После поиска появится окно аутентификации, игнорируем его.
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/40274e25-60c1-4dc4-a084-bedc97d1e8de)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/cb9e9e41-98d4-4f1f-9a67-7fe77e1eba62)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7c530dac-46c4-4d4a-a286-79ca943cfed5)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/45f7d6a2-9cd6-4728-9587-4af2af385b0f)  

Проверяем результат:  
## **BR-SRV**
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/58b11fc3-e81a-4fff-982e-1fc83ab1697f)  


7. Между офисами HQ и BRANCH установите защищенный туннель, позволяющий осуществлять связь между
регионами с применением внутренних адресов.

## **HQ-R**

```
nano /etc/strongswan/ipsec.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/a54fce7e-53d3-480c-9be3-e1e8f199d8b0)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/65563fc5-13b8-4c36-a392-db77c19ec9fe)  
```
conn gre
auto=start
type=tunnel
authby=secret
left=192.168.0.2
right=192.168.1.2
leftsubnet=0.0.0.0/0
rightsubnet=0.0.0.0/0
leftprotoport=gre
rightprotoport=gre
ike=aes256-sha2-256=modp1024!
esp=aes256-sha_256!
```

```
systemctl enable --now ipsec.service
```
## **BR-R**
```
nano /etc/strongswan/ipsec.conf
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/124af0e2-aaac-41da-a80b-387da03724bf)  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/971cd09f-9ccd-4245-92de-9f7ce9521615)  

```
systemctl enable --now ipsec.service
```

```
ipsec status
```

9. Настройте программный RAID 5 из дисков по 1 Гб, которые подключены к машине BR-SRV.

## **BR-SRV**

```
lsblk
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/2e20c3f2-2ece-4229-832e-a288622b8ede)  

```
mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd
watch cat /proc/mdstat
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/777719db-054f-4c0d-b444-94fc894ae1cc)  

```
mkfs.ext4 /dev/md0
mkdir /mnt/raid5
mount /dev/md0 /mnt/raid5
nano /etc/fstab
```
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/c4fcf214-b4ef-467f-8523-808804b407d1)  

Проверяем работоспособность:  
![image](https://github.com/NyashMan/DEMO2024/assets/1348639/7875afe5-027e-46ca-aaa4-85ed688f0528)  
