> **Ubuntu 20.04 base config**

> Первоначальная настройка SSH сервера 

- Создаем пользователя и даем ему права на sudo 

`adduser <USER>`
создаем нового пользователя 

`usermod -aG sudo newuser`
добавляем его в группу sudo 


- Меняем default порт на ssh-сервере

`sudo nano /etc/ssh/sshd_config`

`Port 22`

меняем на

`Port 3333`

- Запрещаем авторизацию для рута

`sudo nano /etc/ssh/sshd_config`

`PermitRootLogin yes`

меняем на

`PermitRootLogin no`

`systemctl restart sshd`
рестартуем sshd



> Настройка SSH key auth

1.	генерим новые ключи
`ssh-keygen`

- Для Lin хоста через 
`ssh-copy-id username@remote_host`
копируем сгенерированные ключи на сервер

- Для Win машины
`TYPE C:\Users\Alex\.ssh\id_rsa.pub`
выводим содержимое файла на локальном хосте 

2.	Подключаемся по SSH к удаленной машине

`mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys && chmod -R go= ~/.ssh` 
создаем файл для ключа авторизации

`nano ~/.ssh/authorized_keys`
пишем публичный ключ, который мы получили на локальном хосте Win машины 

`ssh -i "C:\Users\Alex\.ssh\id_rsa" <USER>@<IP> -p 3333`
пытаемся подключиться на машину

`sudo nano /etc/ssh/sshd_config`
выключаем доступ аутентификацию по паролю

`PasswordAuthentication yes`

меняем на 

`PasswordAuthentication no`

`systemctl restart sshd`
рестартуем сервис 


> Настраиваем UFW

`sudo ufw status`

`sudo ufw enable`

`sudo ufw default deny incoming`
запрещаем default policy для входящих пакетов

`sudo ufw default allow outgoing`
разрешаем defaul policy для исходящих пакетов 

`sudo ufw allow 3333/tcp`
разрешаем ssh на 3333

`sudo ufw limit 3333/tcp`
ограничиваем подключения с одного адреса (перебор паролей).
если больше 6 подключений за 30 секунд

`sudo ufw status verbose`
если надо посмотреть подробнее политики и что мы натворили




