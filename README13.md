# Домашнее задание к занятию 7 «Жизненный цикл ПО»

## Основная часть

Необходимо создать собственные workflow для двух типов задач: bug и остальные типы задач. Задачи типа bug должны проходить жизненный цикл:

1. Open -> On reproduce.
2. On reproduce -> Open, Done reproduce.
3. Done reproduce -> On fix.
4. On fix -> On reproduce, Done fix.
5. Done fix -> On test.
6. On test -> On fix, Done.
7. Done -> Closed, Open.

Остальные задачи должны проходить по упрощённому workflow:

1. Open -> On develop.
2. On develop -> Open, Done develop.
3. Done develop -> On test.
4. On test -> On develop, Done.
5. Done -> Closed, Open.

**Что нужно сделать**

1. Создайте задачу с типом bug, попытайтесь провести его по всему workflow до Done. 
1. Создайте задачу с типом epic, к ней привяжите несколько задач с типом task, проведите их по всему workflow до Done. 
1. При проведении обеих задач по статусам используйте kanban. 
1. Верните задачи в статус Open.
1. Перейдите в Scrum, запланируйте новый спринт, состоящий из задач эпика и одного бага, стартуйте спринт, проведите задачи до состояния Closed. Закройте спринт.
2. Если всё отработалось в рамках ожидания — выгрузите схемы workflow для импорта в XML. Файлы с workflow и скриншоты workflow приложите к решению задания.


**Решение**

Получились следующие схемы workflow
![Image alt](https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/For%20bugs.png)
https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/For%20bugs.xml
![Image alt](https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/For%20others.png)
https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/For%20others.xml

Так выглядит kanban
![Image alt](https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/Kanban.png)

Отчет о выполнении спринта
![Image alt](https://github.com/sibrael/Netology/blob/74a20bf1af044c285850c41de7dc5ae6d8271a8f/Sprint_Reports.png)




---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.

---
