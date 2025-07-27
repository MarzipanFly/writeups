«Уязвимости обхода аутентификации» 
  Скачал файл zip с уязвимым веб-приложением и поднял его у себя с помощью команды docker-compose up -d
Переходим по ссылке http:/localhost:1337

<img width="1911" height="1039" alt="image" src="https://github.com/user-attachments/assets/b2ad8458-deb7-4681-bb99-f9e60a6b25ba" />

  После изучил все возможные директрии простой командой, встроенной в Kali Linux под названием dirb

<img width="535" height="602" alt="image" src="https://github.com/user-attachments/assets/fe8ccace-e552-43f0-8f1a-88269b30e908" />

  Здесь мы видим интерсную сслыку http://127.0.0.1:1337/admin/index.php. Переходим по этой ссылке и видим форму аутентификации

<img width="1906" height="1039" alt="image" src="https://github.com/user-attachments/assets/f805ae16-157b-48c4-b245-5ca5d6e1c4c9" />

  Как известно из задания ранее логин "admin". Поробуем ввести "admin:admin" 

<img width="1917" height="997" alt="image" src="https://github.com/user-attachments/assets/538ae480-46e4-4f9b-b9cc-6b076d008092" />

  Пароль неврный, но зато нам известен логин. Это упрощает нам задачу, так как нам надо только подобрать пароль для входа. Для брут форса я воспользовался встроенной утилитой "hydra". Команда выглядит следующим образом hydra -l admin -P /usr/share/wordlists/rockyou.txt -s 1337 "http-form-post://localhost/admin/:username=^USER^&password=^PASS^:Wrong password. Try again.", где в параметре -l указывается логин, параметр -P указывает, что будет использоваться файл со списком паролей, в моеём случае это уже заранее подготовленный словарь от Kali, который предварительно надо разархивировать, -s указывает на прослушиваемый порт, дальшше соответсвенно два параметра USER и PASSWORD, куда hydra будет подставлять предоставленные нами данные и в конце сообщение, которые выводитс япри неверном введении пароля. Результат работы представлен ниже 

<img width="1675" height="208" alt="image" src="https://github.com/user-attachments/assets/25ea00f3-d3df-4794-a833-e073523985c2" />

  Креды от админской панели "admin:q1w2e3r4". Вводим и мы успешно попадаем на следующий рубеж защиты.

<img width="1888" height="981" alt="image" src="https://github.com/user-attachments/assets/c24c487d-9dae-468b-8484-33818d166de4" />

  Дальше я воспольщовался утилитой Burp Suite. как и hydra данная утилита предустановлена на Kali Linux. Отправляем любой код (в моём случае 123) и в разделе proxy утилиты burp Suite перехватываем запрос.

<img width="1594" height="892" alt="image" src="https://github.com/user-attachments/assets/edc63123-f4c5-4575-9a6b-5799cc441412" />

  На скриншоте виден параметр OTP с моим числом. Отправляем данный запрос в intruder с помщью комбинации клавиш <control+I>. Значение 123 мы обособляем амберсантами при помощи кнопки <Add §>. В разделе payloads выбираем тип paylods "number". Задаём значения от 1 до 1000 с шагом 1.

<img width="1595" height="898" alt="image" src="https://github.com/user-attachments/assets/d1535f41-a694-4554-a70a-02095bf6809d" />

  Запускаем атаку и ищем статус код "302", котоырй уведомляет нас об успешном переходе на страницу. Я воспользовался фильтром и вот что я вижу.

<img width="1475" height="840" alt="image" src="https://github.com/user-attachments/assets/bc0529d1-2f1a-4b0b-bb71-b24c10707ffe" />

  Пробуем ввести 302 и ввидим, как мы успешнопопадаем на администраторскую панель.

<img width="1914" height="1026" alt="image" src="https://github.com/user-attachments/assets/3e2ee08d-3037-436b-96e1-02005b5c3f1f" />

  В попытках поиска флага пробуем изучить код страницы и в самом внизу вы видим в комментариях наш флаг. Проверяем флаг и радуемся успешному завершению задания.

<img width="936" height="696" alt="image" src="https://github.com/user-attachments/assets/51d114df-19d9-41cd-9498-42155f0225f8" />



