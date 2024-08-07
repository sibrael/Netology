**Задание 1. Резервное копирование**

Скачайте и установите виртуальную машину Metasploitable: https://sourceforge.net/projects/metasploitable/.  
Это типовая ОС для экспериментов в области информационной безопасности, с которой следует начать при анализе уязвимостей.  
Просканируйте эту виртуальную машину, используя nmap.  
Попробуйте найти уязвимости, которым подвержена эта виртуальная машина.  
Сами уязвимости можно поискать на сайте https://www.exploit-db.com/.  
Для этого нужно в поиске ввести название сетевой службы, обнаруженной на атакуемой машине, и выбрать подходящие по версии уязвимости.  
Ответьте на следующие вопросы:  
1. Какие сетевые службы в ней разрешены?  
2. Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)  



**Решение 1**  

1.1 Запущенные службы  

![Image alt](https://github.com/sibrael/Netology/blob/9c1088c5faa63912f9fa889364a786a46687b4be/IB_1.png)
https://github.com/sibrael/Netology/blob/9c1088c5faa63912f9fa889364a786a46687b4be/IB_1.png

1.2 Уязвимости

![Image alt](https://github.com/sibrael/Netology/blob/9c1088c5faa63912f9fa889364a786a46687b4be/IB_1_1.png)
![Image alt](https://github.com/sibrael/Netology/blob/9c1088c5faa63912f9fa889364a786a46687b4be/IB_1_2.png)
![Image alt](https://github.com/sibrael/Netology/blob/9c1088c5faa63912f9fa889364a786a46687b4be/IB_1_3.png)

---

**Задание 2. PostgreSQL**

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.  
Запишите сеансы сканирования в Wireshark.    
Ответьте на следующие вопросы:    
1. Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
2. Как отвечает сервер?     

  
 **Решение 2**   
 
Записи:  
SYN - https://disk.yandex.ru/d/vs2bYmOC5oscOQ  
Xmas - https://disk.yandex.ru/d/k4u3msl9sxBe2A  
UDP - https://disk.yandex.ru/d/IACwQjbko676DQ   
FIN - https://disk.yandex.ru/d/xhcocpy1om4K8A  

TCP SYN Scan (TCP SYN) — при использовании этого метода Nmap отправляет пакеты TCP SYN вместо полного TCP подключения. Этот метод быстрее, чем TCP Connect Scan, так как не требует установки соединения. Однако некоторые системы могут блокировать такие пакеты.  

UDP Scan (UDP) — этот метод используется для сканирования UDP портов. Nmap отправляет datagram packets на каждый порт и проверяет, есть ли ответ. Это медленный процесс, потому что многие UDP порты не отвечают на запросы.  

FIN, X-mas, Null Scan (FIN, XMAS, NULL) — эти методы используются для проверки наличия открытых портов на удалённом хосте. Они отправляют специальные TCP пакеты (FIN, XMAS, NULL) на каждый порт и проверяют, есть ли ответ. Эти методы могут быть обнаружены системами IDS/IPS.  


---
