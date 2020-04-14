# Задание D6.11

   Проект домашней библиотеки. Функционал включает следующие моменты:

  - Через панель администратора: возможно добавление, изменение и удаление книг, авторов, друзей, издательств, обложек книг;
  - Без администратора возможно добавление и изменение книг, авторов, друзей, изданий, обложек книг.

Для того, чтобы запустить локальный сервер необходимо:
1) Распакуйте проект в папку C:\my_site
2) Откройте командную строку и зайдите в директорию проекта:
   - cd C:\my_site
3) Создате виртуальное окружение: -- каталог django не закоммичен на GitHab , команда обязательная-- 
   - python -m venv django
4) Активируйте виртуальное окружение:
   - django\Scripts\activate.bat
5) Установите все необходимые пакеты: -- если после ДЗ D5 менялось окружение
   - pip install -r requirements.txt
6) Запустите локальный сервер:
   - python manage.py runserver
Сервер поднимается по адресу
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.

админка по адресу http://127.0.0.1:8000/admin/
все остальные страницы доступны через навигацию по сайту 
в базу добавлен новый автор и его книга 

Для того, чтобы сделать деплой на heroku необходимо:
1) Изменить в settings.py:
	Все эти настройки меняются только для внешнего сайта - для внутреннего 127.0.0.1 все вернуть всё назад
   - SECRET_KEY = 'Ваш_секретный_код' на SECRET_KEY = os.environ.get('SECRET_KEY') как это было рассказано в скринкасте Владимира Ваганова
   - ALLOWED_HOSTS = ['*'] на ALLOWED_HOSTS = ['127.0.0.1','localhost','0.0.0.0'] в скринкасте есть еще адрес на heroku
   - DATABASES = {'default': {'ENGINE': 'django.db.backends.sqlite3','NAME': os.path.join(BASE_DIR, 'db.sqlite3'),}} на import dj_database_url и DATABASES = {'default': dj_database_url.config(default=os.environ['DATABASE_URL'])}
   - STATIC_URL = '/static/' на STATIC_URL = '/asset-v1:SkillFactory+PWS-1+5JUN2019+type@asset+block@/' и STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
ВСЕ Изменения со стандарта Django описаны комментариями в тексте файла settings.py
1/1) настройки Heroku 37:00 скринкаста 
	- Входим в Heroku
	- подключаем Heroku Postgres
	- настроить интеграцию с GitHub
	- Procfile 42:49
	- fixtures 43:54
	- медиа файлы DEBUG=True - иначе медиа файлы видны не будут
	- heroku config:set DISABLE_COLLECTSTATIC=1
	- python manage.py collecstatic --noinput
	- смотрим Deploy --> Deploy Branch
https://devcenter.heroku.com/articles/getting-started-with-python#prepare-the-app
2) Перейти в каталог с проектом:
   - cd C:\my_site
3) Выпонить следующие команды:
   - git init
   - git add .
   - git commit -m "initial commit
   - heroku login
   - heroku create
   - heroku addons:create heroku-postgresql --as DATABASE
   - heroku config:set SECRET_KEY=Ваш_секретный_код
   - git push heroku master
   - heroku run python manage.py loaddata data.xml
   - heroku run python manage.py createsuperuser
4) Если необходимо переименовываем приложение:
   - heroku rename -a oldname newname
5) Запускаем приложение:
   - heroku open
