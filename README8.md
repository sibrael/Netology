**Задание 1. Резервное копирование**

Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования.  
Необходимо описать, какие варианты резервного копирования подходят в случаях:  
1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.  
1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.  
1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.    


**Решение 1**  

1.1 Для данного случая я бы порекомедовал дифференциальный бэкап, как наиболее удобный и надеждый способ резервирования. При ежедневном создании накопительной копии и еженедельном полном бэкапе можно достаточно быстро восстановить данные из полной еженедельной копии.  
1.2 В данном случае возможно есть смысл ежечасно делать инкрементальные бэкапы, но данная процедура будет весьма трудозатратна. Горячий резерв в данном случае не применим, так как с высокой вероятсностью сбой который случится в остановной БД может с лёгкостью реплицироваться в резерв и сломать его тоже.  
1.3 А вот в данном случае как раз таки подходит горячий резерв, при соответствующий настройке мониторинга можно сделать триггер по которому при получении отрицательного health check'а от основной БД система инициирует переключение на резерв и обратно при получении информации от мониторинга, о том, что работоспособность основной БД восстановлена.    


---

**Задание 2. PostgreSQL***

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).  
2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?     

  
 ***Решение 2***  
2.1 
В PostgreSQL есть несколько команд для резервного копирования и восстановления базы данных. Вот некоторые из них:  
Команда для создания резервной копии всей базы данных:  

_BACKUP DATABASE my_database TO '/path/to/backup';_
Эта команда создает полную резервную копию базы данных my_database и сохраняет ее в файл /path/to/backup.  

Команда для создания резервной копии определенной таблицы:  

_BACKUP TABLE my_table TO '/path/to/backup';_ 
Эта команда создает резервную копию таблицы my_table и сохраняет ее в файл /path/to/backup.  

Команда для восстановления базы данных из полной резервной копии:  

_RESTORE DATABASE my_database FROM '/path/to/backup';_  
Эта команда восстанавливает базу данных my_database из полной резервной копии, расположенной в файле /path/to/backup.  

Команда для восстановления таблицы из резервной копии:  

_RESTORE TABLE my_table FROM '/path/to/backup';_  
Эта команда восстанавливает таблицу my_table из резервной копии, расположенной в файле /path/to/backup.  

2.2 Делать резервные копии можно с помощью различных планировщиков, запускать скрипты по расписанию.
Для автоматизации процесса создания резервных копий в PostgreSQL можно использовать различные инструменты и методы. Вот несколько вариантов:

**Crontab**: Это стандартный инструмент Unix-подобных систем для планирования задач. Вы можете создать crontab задачу, которая будет запускать скрипт для создания резервной копии базы данных в определенное время.  
**pg_dump**: Это утилита командной строки, которая позволяет создавать резервные копии баз данных PostgreSQL. Вы можете использовать cron для автоматического запуска pg_dump в нужное время.  
**Continuous Integration/Continuous Deployment (CI/CD)**: Если вы работаете в среде CI/CD, вы можете настроить задачи для создания резервных копий перед каждым развертыванием или после него.  
**WAL-G**: Это инструмент для резервного копирования и репликации PostgreSQL, который поддерживает автоматическое создание резервных копий с помощью cron или других планировщиков.  
**Barman**: Это еще один инструмент для резервного копирования и восстановления PostgreSQL, который также поддерживает автоматическое создание резервных копий.  
**Patroni**: Это оркестратор для PostgreSQL, который может использоваться для автоматизации процессов резервного копирования и восстановления.  
**Time-based Replication**: Если у вас уже настроена репликация между серверами PostgreSQL, вы можете настроить основной сервер так, чтобы он регулярно отправлял полный дамп своей базы данных на вторичные серверы. Таким образом, у вас всегда будут актуальные резервные копии.  


---

**Задание 3. MySQL**

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL.

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?   

  
 **Решение 3**            
3.1
Пример команды для инкрементного резервного копирования в MySQL:


_mysqldump --opt --incremental --single-transaction --databases database_name > incremental_backup.sql_  
В этой команде:

--opt включает все опции оптимизации.  
--incremental указывает, что это инкрементная резервная копия.  
--single-transaction обеспечивает, что резервная копия будет сделана в рамках одной транзакции, что важно для согласованности данных.  
--databases указывает, что нужно сделать резервную копию всех баз данных, перечисленных после этого параметра. В данном случае мы делаем бэкап только одной базы данных database_name.  
-- знак больше используется для перенаправления вывода команды в файл incremental_backup.sql.

3.2
Использование реплики может дать преимущества по сравнению с обычным резервным копированием в следующих случаях:

**Быстрое восстановление**: Репликация позволяет быстро восстановить данные в случае сбоя или потери информации. Если у вас есть реплика, вы можете просто переключиться на нее и продолжить работу без длительного ожидания восстановления из резервной копии.   
**Меньше времени простоя**: При использовании реплики время простоя системы значительно сокращается, так как нет необходимости ждать завершения процесса восстановления из резервной копии.  
**Улучшенная доступность**: Репликация обеспечивает высокую доступность данных, так как она предоставляет возможность одновременного доступа к данным с нескольких серверов.  
**Масштабируемость**: Репликация позволяет масштабировать систему путем добавления новых узлов репликации, что увеличивает производительность и надежность системы.  
**Уменьшение нагрузки на основной сервер**: Репликация помогает распределить нагрузку на несколько серверов, что уменьшает нагрузку на основной сервер и повышает его производительность.  
**Упрощение обновления и миграции**: Репликация упрощает процесс обновления и миграции системы, так как изменения могут быть легко распространены на все реплики.  
**Защита от отказов**: Репликация защищает от отказов основного сервера, так как данные хранятся на нескольких серверах.  
**Контроль версий**: Репликация позволяет контролировать версии данных, что облегчает процесс восстановления данных до определенной версии.

---