# 📄 Cyber Team HW6

В данном домашнем задании выполнены задачи по работе с хэшами, подбору PIN-кодов, атаке с солью, работе с MD5 и настройке HTTPS.

---

## 🔹 Задание 1 – Определение типа хэша и взлом

Сначала был определён тип хэшей по длине и формату. Выполнялись попытки с разными алгоритмами (SHA256, SHA512), после чего был выбран корректный формат.

Команды:
john hashes_john.txt --wordlist=rockyou.txt  
john --show hashes_john.txt  

Процесс подбора и проверки формата:

![john-hash](images/john-hash.png)
![johnformat](images/johnformat.png)
![trysha256](images/trysha256.png)
![trysha256v2](images/trysha256v2.png)
![testssh256](images/testssh256.png)
![512ssh](images/512ssh .png)
![ssh512](images/ssh512|5|6.png)

Результат:

![johnshowres](images/johnshowres.png)

---

## 🔹 Задание 2 – PIN-коды (mask attack)

Был выполнен перебор PIN-кодов с использованием масок.

Команды:
john --format=Raw-SHA512 --mask=?d?d?d?d pins.txt  
john --format=Raw-SHA512 --mask=?d?d?d?d?d pins.txt  
john --format=Raw-SHA512 --mask=?d?d?d?d?d?d pins.txt  

Также выполнялась проверка длины:

![countlength](images/countlenght.png)

Редактирование и подготовка:

![sortedpins](images/sortedpinsnano.png)

В процессе были обнаружены ошибки в хэшах:

![brokenhash](images/brokenhash512.png)
![brokenhashL](images/brokenhasherror"l".png)
![fix0](images/findfix0.png)

Результаты перебора:

![johnshow512](images/johnshow512results.png)
![finalresult](images/finalresult5.png)

Вывод: найдено 4 PIN-кода, один не найден после полного перебора диапазона.

---

## 🔹 Задание 3 – Salted Hashes

Формат входных данных:
login:salt:hash  

Использовались форматы:

dynamic_61 → sha256(password + salt)  
dynamic_62 → sha256(salt + password)  

Выполнялись попытки с обоими форматами:

![61try](images/61trysalt.png)
![62try](images/62trysalted.png)

Итоговые результаты:

![61result](images/61result.png)
![62result](images/62result.png)

Вывод: корректный формат определён, пароли успешно восстановлены.

---

## 🔹 Задание 4 – MD5 (словарная атака)

Тип хэша определён по длине:

awk '{print length, $0}' rainbow.txt  

Длина = 32 → MD5  

Использован словарь rockyou.txt.

Результаты:

![rainbowcount](images/ranbowsortcount.png)
![rainbowresult](images/md5resultrainbow.png)

Вывод: пароли успешно восстановлены.

---

## 🔹 Задание 5 – HTTPS без ошибок

Был настроен HTTPS-сервер с самоподписанным сертификатом.

Создание сертификата:

![setupcert](images/setupcerts.png)



После добавления сертификата в доверенные:

sudo cp ~/certs/cert.pem /usr/local/share/ca-certificates/localhost.crt  
sudo update-ca-certificates  

Финальный результат:

![httpsok](images/resultcertshttpsworks.png)

Вывод: HTTPS работает без ошибок SSL.

---
