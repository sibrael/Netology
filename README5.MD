**Задание 1**

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.
select * from sakila.address a  
where a.district like 'C%a'  
and a.district not like '% %'
  
![Image alt](https://github.com/sibrael/Netology/blob/9097e7215f0ea4156625af950e7dc4c5b4746df4/MySQL1.png)


---

**Задание 2**

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно и стоимость которых превышает 10.00.
select * from sakila.payment p  
where payment_date between '2005-06-15 00:00:00' and '2005-06-18 23:59:59'
and amount > 10
             
![Image alt](https://github.com/sibrael/Netology/blob/9097e7215f0ea4156625af950e7dc4c5b4746df4/MySQL1.png)


---

**Задание 3**

Получите последние пять аренд фильмов.
  
![Image alt](https://github.com/sibrael/Netology/blob/9097e7215f0ea4156625af950e7dc4c5b4746df4/MySQL1.png)


---

**Задание 4**

Одним запросом получите активных покупателей, имена которых Kelly или Willie.

Сформируйте вывод в результат таким образом:

все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
замените буквы 'll' в именах на 'pp'.
  
![Image alt](https://github.com/sibrael/Netology/blob/9097e7215f0ea4156625af950e7dc4c5b4746df4/MySQL1.png)


---

