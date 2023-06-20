```
Kittygram
Блог для любителей котиков.
```
Технологии
Python 3.x
node.js 9.x.x
frontend: React
backend: Django
nginx
gunicorn
```
Установка
Все примеры указаны для Linux
```
1. Склоинровать репозиторий
```
git@github.com:AnnaKotovapr/infra_sprint1.git
```
2. Создать и активировать виртуальное окружение
```
cd infra_sprint1
python3 -m venv venv
source venv/bin/activate
```
3. Установить зависимости для Python
```
pip install -r requirements.txt 
```
4. Перейти в папку backend и выполнить миграции
```
cd infra_sprint1/backend/
python manage.py migrate
```
5. Создать суперюзера
```
python manage.py createsuperuser
```
6. В файле infra_sprint1/backend/kittygram_backend/settings.py в переменную ALLOWED_HOSTS добавить локальные адреса, а также доменное имя или внешний IP (если есть)
```
ALLOWED_HOSTS = ['127.0.0.1', '0.0.0.0', 'localhost', 'xxx.xxx.xxx.xxx']
```
7. В этом же файле поменять значение переменной DEBUG с True на False
```
DEBUG = False
```
```
8. Перейти в infra_sprint1/frontend и установить зависимости для frontend-приложения
```
npm i
```
9. Для backend-приложения установить WSGI-сервер gunicorn
```
pip install gunicorn
```
11. Создать файл конфигурации для автозапуска WSGi-сервера
```
sudo nano /etc/systemd/system/gunicorn_kittygram.service
```
12. Запустить созданную службу и внести её в автозапуск
```
sudo systemctl start gunicorn_kittygram
sudo systemctl enable gunicorn_kittygram
```
13. Установить веб-сервер и запустить его
```
sudo apt install nginx -y
sudo systemctl start nginx 
```
14. Открыть порты для фаерволла и активировать его
```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```
15. Перейти в infra_sprint1/frontend/, скомпилировать frontend-приложение и копировать его в системную директорию веб-сервера (необходимо для корректной работы статики)
```
npm run build
sudo cp -r <имя_пользователя>/infra_sprint1/frontend/build/. /var/www/infra_sprint1/
```
16. Перейти в infra_sprint1/backend/, собрать статику для админки Django и копировать его в системную директорию веб-сервера (ВАЖНО! Необходимо изменить название папки со статикой во избежание конфликта имён)
```
infra_sprint1/backend/kittygram_backend/settings.py поменять значение переменной STATIC_URL и добавить новую STATIC_ROOT
```
STATIC_URL = 'static_backend'
STATIC_ROOT = BASE_DIR / 'static_backend' 
python manage.py collectstatic
sudo cp -r infra_sprint1/backend/static_backend/ /var/www/infra_sprint1/
```
17. Создать папку для медиафайлов в директории веб-сервера, изменить права доступа
```
cd /var/www/infra_sprint1/
mkdir media
sudo chown -R <имя_пользователя> /var/www/infra_sprint1/media/
```
18. Перейти в файл конфигурации веб-сервера и изменить его настройки
```
19. Проверить файл конфигурации веб-сервера, перезагрузить его конфиг в случае успеха
```
sudo nginx -t
sudo systemctl reload nginx
```
20. Получить SSL-сертификат для вашего доменного имени при помощи certbot
```
Автор:
Анна Котова (Тоже котик)
```

