# Подготовка нового сервера для запуска сайтов
Скачайте файлы репозитория на компьютер.

## Проверка возможности соединения по SSH
Некоторые шаги подготовки сервера далее включают использование Ansible. Он соединяется с сервером через SSH. Поэтому нужно проверить возможность такого подключения.

Если не известно есть ли на клиенте публичный и секретный ключ, то можно посмотреть в их стандартном месте: папке .ssh в домашней директории пользователя. Перейду туда:
```ls ~/.ssh```

Если ключи есть, то увижу два файла: id_rsa (личный) и id_rsa.pub (публичный).
Если ключей нет, то их нужно сгенерировать:
```ssh-keygen```

После этого по указанному пути появятся два файла.
```ls ~/.ssh```

Теперь нужно скопировать публичный ключ на сервер. Он требуется для расшифровки присланной строки. Это делается через команду ssh-copy-id и затем указывается имя пользователя, коммерческое эт и IP или доменное имя сервера.
```ssh-copy-id root@194.93.0.199```

Он сообщит, что на сервере не установлен публичный ключ и уверен ли я в продолжении соединения. Отвечаю «да».

После он спросит пароль от сервера. Ему требуется пароль чтобы понимать, что с клиента зашёл не посторонний человек. Если всё правильно, то в папке .ssh создастся ещё один файл known_hosts где будет указано, что публичный ключ этого компьютера есть на сервере с указанным IP:

Теперь на сервер можно заходить так:
```ssh 'root@194.93.0.199'```

## Установка и настройка Ansible
Чтобы управлять другим сервером через Энсибл его нужно установить на личном компьютере. Для Мака:
```brew install ansible```

Проверка установки
```ansible --version```

Затем подключусь к серверу и проверю наличие Пайтона.
```python3 --version```

Ещё обнови пакеты чтобы в будущем не было ошибок при установке программ через Энсибл.
```sudo apt update```
```sudo apt upgrate```

Проверьте адрес сервера в inventory.ini

## Установка Докера
Установите Докер:
```ansible-playbook -i inventory.ini install-docker-playbook.yml```

## Установка Дженкинса
Установите Докер:
```ansible-playbook -i inventory.ini install-jenkins-playbook.yml```

## Создание папки с сайтами
Установите Докер:
```ansible-playbook -i inventory.ini create-sites-folder.yml```

## Создание главного Nginx
Запустите контейнер с Nginx, который будет связывать запросы на сервер к другим Nginx отвечающих за сайты:
```ansible-playbook -i inventory.ini run-nginx-proxy.yml```