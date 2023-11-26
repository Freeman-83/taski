# Taski
Приложение для планирования своих задач.

## Технологии
- Python
- Django
- Django Rest Framework
- Gunicorn
- Nginx
- ufw
- snapd
- certbot


## Как развернуть проект на сервере под управлением Linux:

Клонируйте репозиторий :

```
git clone <адрес репозитория на GitHub>
```
и перейдите в него.


### Настройка бэкенд-приложения:
Перейдите в папку с проектом, создайте и активируйте виртуальное окружение:
```
python3 -m venv env
```
```
source env/bin/activate
```

Установите зависимости из файла requirements.txt:
```
python3 -m pip install --upgrade pip
```
```
pip install -r requirements.txt
```

Выполните миграции:
```
python3 manage.py migrate
```

В корневой папке проекта создайте файл .env для значений SECRET_KEY и DEBUG

Соберите статику бэкенд-приложения:
```
python3 manage.py collectstatic
```

Скопируйте директорию static_backend/ в директорию /var/www/название_проекта/:
```
sudo cp -r путь_к_директории_с_бэкендом/static_backend /var/www/название_проекта
```

### Настройка фронтенд-приложения

Находясь в директории с фронтенд-приложением, установите зависимости для него:

```
npm i
```
и выполните команду:

```
npm run build
```
Скопируйте статику фронтенд-приложения в директорию по умолчанию:

```
sudo cp -r путь_к_директории_с_фронтенд-приложением/build/. /var/www/имя_проекта/
```

### Установка и настройка WSGI-сервера Gunicorn

Перейдите в директорию с файлом manage.py, и запустите Gunicorn:
```
gunicorn --bind 0.0.0.0:8000 backend.wsgi
```
Создайте файл конфигурации юнита systemd для Gunicorn в директории
/etc/systemd/system/:

```
sudo nano /etc/systemd/system/gunicorn_название_проекта.service
```

### Установка и настройка веб- и прокси-сервера Nginx

Если Nginx ещё не установлен на удалённый сервер, установите его:
```
sudo apt install nginx -y
```
Запустите Nginx:
```
sudo systemctl start nginx
```

Откройте файл конфигурации веб-сервера и обновите настройки Nginx:
```
sudo nano /etc/nginx/sites-enabled/default
```
Перезагрузите конфигурацию Nginx:
```
sudo systemctl reload nginx
```

### Настройка файрвола ufw

Активируйте разрешение принимать запросы только на порты 80, 443 и 22:

```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
```
Включите файрвол:
```
sudo ufw enable
```

### Получение и настройка SSL-сертификата

Находясь на сервере, установите certbot, если он ещё не установлен:
```
sudo apt install snapd
```

Установите и обновите зависимости для пакетного менеджера snap:
```
sudo snap install core; sudo snap refresh core
```

Установите пакет certbot:
```
sudo snap install --classic certbot
```

Обеспечьте доступ к пакету для пользователя с правами администратора:
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

Запустите certbot и получите SSL-сертификат:
```
sudo certbot --nginx
```


## Автор
### Михаил Марин
