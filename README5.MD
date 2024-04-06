**Задание 1**

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

select * from sakila.address a  
where a.district like 'C%a'  
and a.district not like '% %'
  
![Image alt](https://github.com/sibrael/Netology/blob/c844605fe4235f31a36dc6a6f2282441504fee55/MySQL2_1.png)


---

**Задание 2**

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года включительно и стоимость которых превышает 10.00.

select * from sakila.payment p  
where payment_date between '2005-06-15 00:00:00' and '2005-06-18 23:59:59'
and amount > 10
             
![Image alt](https://github.com/sibrael/Netology/blob/c844605fe4235f31a36dc6a6f2282441504fee55/MySQL2_2.png)



---

**Задание 3**

Получите последние пять аренд фильмов.

select * from sakila.rental r   
order by rental_date  desc limit 5  

  
![Image alt](https://github.com/sibrael/Netology/blob/c844605fe4235f31a36dc6a6f2282441504fee55/MySQL2_3.png)


---

**Задание 4**

Одним запросом получите активных покупателей, имена которых Kelly или Willie.

Сформируйте вывод в результат таким образом:

все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
замените буквы 'll' в именах на 'pp'.

select lower(REPLACE(c.first_name, 'LL', 'PP')), lower(REPLACE(c.last_name , 'LL', 'PP'))  from sakila.customer c   
where c.first_name in ('KELLY','WILLIE')   
and c.active = 1
  
![Image alt](https://github.com/sibrael/Netology/blob/c844605fe4235f31a36dc6a6f2282441504fee55/MySQL2_4.png)


---

