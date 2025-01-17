# Домашнее задание к занятию "`Резервное копирование`" - `Травицкий Сергей`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

 - Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
 - Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
 - Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
 - На проверку направить скриншот с командой и результатом ее выполнения

**Исключены только директории, скрытые файлы сохранены**  

*Скриншот 1*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/1.1.png)

*Скриншот 2*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/1.2.png)


---

### Задание 2

 - Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
 - Резервная копия должна быть полностью зеркальной
 - Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
 - Резервная копия размещается локально, в директории /tmp/backup
 - На проверку направить файл crontab и скриншот с результатом работы утилиты.

**Скрипт**
```
#!/bin/sh
date=$(date '+%Y-%m-%d %H:%M:%S')
echo $date > /var/log/cron.log
rsync -ac --delete /home/travitskii/ . /tmp/backup >/dev/null 2>>/var/log/cron.log
if [ $? -eq 0 ]; then
    echo "[$(date)] Резервное копирование успешно выполнено" >> /var/log/cron.log
else
    echo "[$(date)] Ошибка при выполнении резервного копирования" >> /var/log/cron.log

```
*Скриншот 1*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/2.1.png)  

*Скриншот 2*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/2.2.png)  

*Скриншот 3*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/2.3.png)  

*Скриншот 4*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/2.4.png)  

**Искуственно сгенерированая ошибка**  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/2.5.png)   

---

### Задание 3

*скрины работы скрипта с ограничением лимита и без ограничения, так как копировал домашнюю*  
*папку целиком, ограничение поставил 10000 КБ. Для удобства время вывел в консоль*  
*Перед копированием удалял архив, правда и без удаления было заметно медленнеее*
![скриншот](https://github.com/travickiy67/Backup/blob/main/img/3.1.png)  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/3.2.png)  
*Скрипт*  
```
#!/bin/sh
START_TIME=$(date +%s)
date=$(date '+%Y-%m-%d %H:%M:%S')
echo $date > /var/log/cron.log
rsync -ac --bwlimit=10000  --delete /home/travitskii/ . /tmp/backup >/dev/null 2>>/var/log/cron.log
END_TIME=$(date +%s)
difference=$(( $END_TIME - $START_TIME ))
echo "$difference seconds"

```
*Или так:Передача на другую машину, файла*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/3.3.png)  

### Задание 4

*Скрипт*

[скрипт](https://github.com/travickiy67/Backup/blob/main/files/test_baskup.sh)

`Скрипт реально рабочий, чтото сам, где интернет. Заработал, но директиву --link-dest заставить работать не получилось, может`  
`потому, что я не понял концепцию, может она просто не работает. В интернете не удалось найти рабочий скрипт с этой командой.`  
`Скрины выполнения скрипта`  

*Первый запуск без ошибки, последующие ругается на --link-dest*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/4.1.png)  

*Бэкапы*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/4.2.png)  

*Востанавливает*  

![скриншот](https://github.com/travickiy67/Backup/blob/main/img/4.3.png)  
