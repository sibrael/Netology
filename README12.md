**Задание 1.**

1. Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
2. Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
3. Выполните код проекта. Найдите в state-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.
5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

**Решение 1** 

1. 

2. В файле personal.auto.tfvars  
3. "result": "Ss1sMFFf0IhLomZZ"
4. В блоке resource "docker_image" отсутствовало имя ресурса, а в блоке resource "docker_container" было нарушено именование ресурса, ресурс не должен начинаться с цифры. Также была ошибка в имени random_password.random_string_FAKE.resulT.

5.
![Image alt](https://github.com/sibrael/Netology/blob/3f2d5d60e6b608c9ab4f0d3c4ff2e9d769fd0cb2/Ter4.png)
![Image alt](https://github.com/sibrael/Netology/blob/44b03dfa58a262a23fac28b64217cd0cdc1b2f08/Ter1.png)

6. Опасность в том, что мы не видим того, что меняет наш код, при ошибке мы автоматически можем сломать инфраструктуру. Применять автоапрув есть смысл в автоматизации, когда мы проводим действия скриптами, чтобы не прописывать команды. Когда изменения проводятся автоматически, рутинные вещи, которые не могут повлиять серьезно на работоспособность всей инфраструктуры.

![Image alt](https://github.com/sibrael/Netology/blob/44b03dfa58a262a23fac28b64217cd0cdc1b2f08/Ter2.png)

7.
![Image alt](https://github.com/sibrael/Netology/blob/44b03dfa58a262a23fac28b64217cd0cdc1b2f08/Ter3.png)

8. keep_locally = true
   
   keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.
 

---
