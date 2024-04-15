**Задание 1**

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.

select s2.last_name, s2.first_name, c2.city, count(c.customer_id) from store s  
join customer c on c.store_id = s.store_id   
join address a on a.address_id = s.address_id   
join city c2 on c2.city_id = a.city_id   
join staff s2 on s2.store_id = s.store_id  
group by s2.first_name, s2.last_name, c2.city  
having count(c.customer_id) > 300  

  
![Image alt](https://github.com/sibrael/Netology/blob/dea86b5041f9f751d837920f68256eb4d9091865/MySql3_1.png)


---

**Задание 2**

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

select  count(f.film_id)  from film f  
where f.`length` > (  
select avg(f.length) from film f )  
             
![Image alt](https://github.com/sibrael/Netology/blob/dea86b5041f9f751d837920f68256eb4d9091865/MySql3_2.png)



---

**Задание 3**

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

select sum(p.amount),count(p.payment_id)  from payment p  
group by EXTRACT(MONTH FROM p.payment_date)  
order by sum(p.amount) desc  limit 1  

  
![Image alt](https://github.com/sibrael/Netology/blob/dea86b5041f9f751d837920f68256eb4d9091865/MySql3_3.png)


---


