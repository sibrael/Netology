**Задание 1**  

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

sudo nmap -sA < ip-адрес >

sudo nmap -sT < ip-адрес >

sudo nmap -sS < ip-адрес >

sudo nmap -sV < ip-адрес >



**Решение 1**  

1.1 Логи suricata 

![Image alt](https://github.com/sibrael/Netology/blob/46bdf2fbf5ceb7d17737b6ab301a0460e2c89670/IB_2_1.png)
![Image alt](https://github.com/sibrael/Netology/blob/46bdf2fbf5ceb7d17737b6ab301a0460e2c89670/IB_2_2.png)
![Image alt](https://github.com/sibrael/Netology/blob/46bdf2fbf5ceb7d17737b6ab301a0460e2c89670/IB_2_3.png)

---

**Задание 2**

Проведите атаку на подбор пароля для службы SSH:

hydra -L users.txt -P pass.txt < ip-адрес > ssh

Настройка hydra:
создайте два файла: users.txt и pass.txt;
в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.
Дополнительная информация по hydra: https://kali.tools/?p=1847.

Включение защиты SSH для Fail2Ban:
открыть файл /etc/fail2ban/jail.conf,
найти секцию ssh,
установить enabled в true.
  
 **Решение 2**   
 
Логи Fail2ban

![Image alt](https://github.com/sibrael/Netology/blob/46bdf2fbf5ceb7d17737b6ab301a0460e2c89670/IB_2_4.png)


---
