**Задание 1**

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.
количество пользователей, закреплённых в этом магазине.

SELECT SUM(index_length * 1.0 / data_length) AS index_percentage  
FROM information_schema.tables  
WHERE table_schema = 'sakila';   


![Image alt](https://github.com/sibrael/Netology/blob/8902186b97258d9e2556f9cbbbeaacaae8447fbe/Index_1.png)


---

**Задание 2**

Выполните explain analyze следующего запроса:  

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)    
from payment p, rental r, customer c, inventory i, film f    
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id    

перечислите узкие места;  
оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.   

  
 **Ответ 2**            
-> Table scan on <temporary>  (cost=2.5..2.5 rows=0) (actual time=2411..2411 rows=391 loops=1)  
    -> Temporary table with deduplication  (cost=0..0 rows=0) (actual time=2411..2411 rows=391 loops=1)  
        -> Window aggregate with buffering: sum(payment.amount) OVER (PARTITION BY c.customer_id,f.title )   (actual time=1514..2298 rows=642000 loops=1)  
            -> Sort: c.customer_id, f.title  (actual time=1514..1555 rows=642000 loops=1)  
                -> Stream results  (cost=21.6e+6 rows=15.6e+6) (actual time=2.93..1125 rows=642000 loops=1)  
                    -> Nested loop inner join  (cost=21.6e+6 rows=15.6e+6) (actual time=2.82..983 rows=642000 loops=1)  
                        -> Nested loop inner join  (cost=20.1e+6 rows=15.6e+6) (actual time=2.01..863 rows=642000 loops=1)  
                            -> Nested loop inner join  (cost=18.5e+6 rows=15.6e+6) (actual time=1.75..741 rows=642000 loops=1)  
                                -> Inner hash join (no condition)  (cost=1.54e+6 rows=15.4e+6) (actual time=0.66..32.7 rows=634000 loops=1)  
                                    -> Filter: (cast(p.payment_date as date) = '2005-07-30')  (cost=1.61 rows=15400) (actual time=0.0253..4.2 rows=634 loops=1)  
                                        -> Table scan on p  (cost=1.61 rows=15400) (actual time=0.0154..3.04 rows=16044 loops=1)  
                                    -> Hash  
                                        -> Covering index scan on f using idx_title  (cost=103 rows=1000) (actual time=0.295..0.549 rows=1000 loops=1)  
                                -> Covering index lookup on r using rental_date (rental_date=p.payment_date)  (cost=1 rows=1.01) (actual time=699e-6..0.00103 rows=1.01 loops=634000)  
                            -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=0.001 rows=1) (actual time=84.7e-6..101e-6 rows=1 loops=642000)  
                        -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=0.001 rows=1) (actual time=81e-6..97.1e-6 rows=1 loops=642000)  
                        
Избыточный и медленный селект.  
Зачем нам выбирать всю информацию из из всех таблиц и организовывать связи через обычные поля (p.payment_date = r.rental_date), когда можно сделать через join'ы и связаться через ключи?  
Также, насколько я знаю, partition by работает чуть медленнее чем group by.  

Моя версия данного запроса  

select concat(c.last_name, ' ', c.first_name) as FIO, sum(p.amount)   
from payment p   
join rental r on r.rental_id =p.rental_id   
join customer c  on c.customer_id = r.customer_id   
join inventory i on i.inventory_id =r.inventory_id  
join film f on f.film_id = i.film_id   
where date(p.payment_date) = '2005-07-30'  
group by c.customer_id, f.title  
order by concat(c.last_name, ' ', c.first_name)    



V1.1  
Film я оставил потому что для вывода важно разделение суммы по фильмам, если на выходо оно не нужно, то поле убрал.
Индекс добавил. План запроса на картинке, аналайз ниже

select concat(c.last_name, ' ', c.first_name) as FIO, sum(p.amount)   
from payment p   
join rental r on r.rental_id =p.rental_id   
join customer c  on c.customer_id = r.customer_id   
join inventory i on i.inventory_id =r.inventory_id  
where p.payment_date >= '2005-07-30'    
and p.payment_date < DATE_ADD('2005-07-30', INTERVAL 1 DAY)  
group by c.customer_id  
order by concat(c.last_name, ' ', c.first_name)  

CREATE INDEX index_date ON payment (payment_date); 

-> Sort: FIO  (actual time=2.69..2.7 rows=391 loops=1)  
    -> Table scan on <temporary>  (actual time=2.55..2.58 rows=391 loops=1)  
        -> Aggregate using temporary table  (actual time=2.55..2.55 rows=391 loops=1)  
            -> Nested loop inner join  (cost=951 rows=634) (actual time=0.029..2.25 rows=634 loops=1)  
                -> Nested loop inner join  (cost=729 rows=634) (actual time=0.0274..1.79 rows=634 loops=1)  
                    -> Nested loop inner join  (cost=507 rows=634) (actual time=0.0246..1.29 rows=634 loops=1)  
                        -> Filter: (p.rental_id is not null)  (cost=286 rows=634) (actual time=0.0189..0.753 rows=634 loops=1)  
                            -> Index range scan on p using index_date over ('2005-07-30 00:00:00' <= payment_date < '2005-07-31 00:00:00'), with index condition: ((p.payment_date >= TIMESTAMP'2005-07-30 00:00:00') and (p.payment_date < <cache>(('2005-07-30' + interval 1 day))))  (cost=286 rows=634) (actual time=0.0181..0.717 rows=634 loops=1)  
                        -> Single-row index lookup on r using PRIMARY (rental_id=p.rental_id)  (cost=0.25 rows=1) (actual time=727e-6..745e-6 rows=1 loops=634)  
                    -> Single-row index lookup on c using PRIMARY (customer_id=r.customer_id)  (cost=0.25 rows=1) (actual time=677e-6..694e-6 rows=1 loops=634)  
                -> Single-row covering index lookup on i using PRIMARY (inventory_id=r.inventory_id)  (cost=0.25 rows=1) (actual time=601e-6..621e-6 rows=1 loops=634)  


![Image alt](https://github.com/sibrael/Netology/blob/88626364ed3700041a19409903b5e0984d590169/Index_2.png)

---
