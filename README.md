# 📄 Cyber Team HW6

В данном домашнем задании выполнены задачи по работе с хэшами, подбору паролей, атаке с солью, использованию аналогов радужных таблиц и настройке HTTPS-сервиса.

## Задание 1 – Базовая работа с хэшами  
Даны хэши, необходимо определить их тип и восстановить пароли. Сначала определяется длина хэша, затем подбирается формат (например MD5 или SHA256), после чего используется словарь rockyou.txt.  

Команды:
john hashes_john.txt --wordlist=rockyou.txt  
john --show hashes_john.txt  

Результат: пароли успешно восстановлены.  

![task1](images/task1.png)

## Задание 2 – Работа с PIN-кодами  
Необходимо было подобрать значения из файла pins.txt. Использовался перебор по словарю.  

Команда:
john pins.txt --wordlist=pins.txt  

Результат: PIN-коды найдены.  

![task2](images/task2.png)

## Задание 3 – Атака с солью (Salted hashes)  
Формат входных данных: login:salt:hash. Важно учитывать порядок: либо пароль+соль, либо соль+пароль.  

Использовались форматы dynamic_61 и dynamic_62.  

Команды:
john --format=dynamic_62 --wordlist=rockyou.txt salted.txt  
john --show salted.txt  

Результат:
web_admin → 656800  
root → rubenkus  
user → 54cami9  
admin → 428248h  

![task3](images/salted.png)

## Задание 4 – Взлом MD5 (аналог rainbow tables)  
Сначала проверена длина хэшей:  

Команда:
awk '{print length, $0}' rainbow.txt  

Длина = 32 → это MD5.  

Далее использован словарь rockyou.txt.  

Команды:
john --format=Raw-MD5 --wordlist=rockyou.txt rainbow.txt  
john --show rainbow.txt  

Результат: пароли успешно найдены.  

![task4_1](images/md5resultrainbow.png)  
![task4_2](images/ranbowsortcount.png)

## Задание 5 – Поднятие HTTPS-сервиса без ошибок  

1. Генерация сертификата:
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes  

2. Настройка nginx:
sudo nano /etc/nginx/sites-available/default  

Добавлено:
listen 443 ssl;  
ssl_certificate /home/kali2/certs/cert.pem;  
ssl_certificate_key /home/kali2/certs/key.pem;  

3. Проверка конфигурации:
sudo nginx -t  
sudo systemctl restart nginx  

4. Первая проверка:
curl https://localhost  
(ошибка self-signed certificate — ожидаемо)  

5. Добавление сертификата в доверенные:
sudo cp ~/certs/cert.pem /usr/local/share/ca-certificates/localhost.crt  
sudo update-ca-certificates  

6. Финальная проверка:
sudo systemctl restart nginx  
curl https://localhost  

Результат:
HTTPS works  

![task5](images/https.png)

