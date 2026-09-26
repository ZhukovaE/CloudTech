[Шпаргалка по разметке mark down](https://gist.github.com/OlgaMurzina/a094b7bfa6f15b25c683309885d68dbb#links)
# 📝 Описание сервиса "Наши заметки"
Мы протестировали локальный запуск сервиса "Наши заметки" на машине с ОС Windows 10 и терминалом GitBash. На других операционных системах и терминалах мы не тестировали запуск и не гарантируем работоспособность сервиса "Заметки".     
## 🛠️ Установка сервиса "Наши заметки"

### Установка на локальной машине

#### Требования. Что нужно для работы сервиса
Для работы сервиса "Наши заметки" на локальной машине должно быть установлено:
1. [Git](https://git-scm.com/download/win). Терминал GitBash станет доступен после установки.
2. [Python3.1x](https://www.python.org/downloads/release/pymanager-263/)
3. [Node.js](https://nodejs.org/en/download).
4. [PostgreSQL.](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
   ВНИМАНИЕ!!! Обязательно запомните пароль суперпользователя postgres. 

#### Действия по установке backend на локальной машине
ВНИМАНИЕ!!! Все команды выполняются в терминале GitBash.
1. Склонируйете репозиторий на локальную машину, для чего в терминале GitBash выполните 
    ```bash
   git clone https://github.com/ZhukovaE/CloudTech.git
    ```   
2. Перейдите в папку ./backend и скопируйте файл .env.example в .env
    ```bash
   cd ./backend/
   cp .env.example .env
   ```
   В файле .env вы увидите строку подключения к базе данных:
   ```ini
   DATABASE_URL=postgresql+psycopg://username:passwor@127.0.0.1:5432/notes_service
    ```   
   Откорректируйте данную строку, заменив username и password на вашего пользователя. На этом этапе можете пока использовать пароль суперпользователя postgres, которые Вы задали при установке PostgreSQL. Например, при установке Вы задали пароль "yourpassword" для суперпользователя postgres. В этом случае, строка подключения к БД примет такой вид:
   ```ini
   DATABASE_URL=postgresql+psycopg://postgres:yourpassword@127.0.0.1:5432/notes_service
    ```   
   где notes_service - название базы данных, с которой будет работать backend.
3. Запустите утилиту pgAdmin и создайте пустую базу данных, с которой будет работать backend.
   В нашем примере база данных называется notes_service.
4. В папке ./backend и создайте виртуальное окружение, в котором будет выполняться backend, активируйте его и установите необходимые пакеты командами:
    ```bash
   python -m venv .venv
   source .venv/Scripts/activate
   python -m pip install -r requirements.txt
   ```
5. Примените миграции на базе данных (должны находиться в папке ./backend):
    ```bash
   flask --app run.py db upgrade
   ```

#### Действия по установке frontend на локальной машине
ВНИМАНИЕ!!! К этому моменту должны быть выполнены действия, описанные в пункте "Действия по установке backend на локальной машине".
1. Перейдите в папку frontend. Если к текущему моменту вы находитесь в папке backend, то выполните:
   ```bash
   cd ../frontend/
   ```
2. В папке ./frontend установите зависимости командой:
    ```bash
   npm install
    ```

## 🚀 Запуск сервиса "Наши заметки"
### Запуск на локальной машине
ВНИМАНИЕ!!! К этому моменту должны быть выполнены действия, описанные в пунктах "Действия по установке backend на локальной машине" и "Действия по установке frontend на локальной машине" 
1. В одном окне терминала GitBash запустите backend командой (должны находиться в папке ./backend):
    ```bash
    flask --app run.py run --port 5100
      ```
2. В другом окне терминала GitBash запустите frontend командой (должны находиться в папке ./frontend):
   ```bash
    npm run dev
   ```
3. В браузере открываем страницу по адресу http://localhost:5173/ и наслаждаемся работой сервиса "Наши заметки"
![img.png](Screenshots/service_page.png)

## 📌 Описание компонентов сервиса "Наши заметки"
### Backend
Backend реализует следующие функции:
1. Реализует логику работы приложения 
2. Предоставляет Web API (Web Application Programming Interface) клиентским приложениям (frontend applications)
3. Реализует взаимодействие с базой данных

Backend реализован на Python с использованием Flask framework. 

В backend реализованы следующие эндпоинты: 
```text
GET    /health
GET    /api/notes
GET    /api/notes/<id>
POST   /api/notes
PATCH  /api/notes/<id>
DELETE /api/notes/<id>
```

# 🛠️ Как это было
## День 1-й. Размышления над задачей
Первая мысль после прочтения задания - всЁ, нам крышка.
Ведь у нас еще не было дисциплины Веб-разработка.

Потом горькие раздумья: что разработать в качестве сервиса и как вообще сделать этот backend и frontend? 

В конце дня вдруг вспомнила, что забыла выкупить таблетки для своей любимой собаки. Пришла идея - сделать сервис для заметок. Надо обсудить с остальными участниками группы.

## День 2-й. Backend начало
Обсудили со всеми участниками группы предложение по разработке сервиса заметок. Были и другие варианты, например кинотеатр. Решили, что заметки будет проще реализовать. Договорились.

Посмотрела что есть на GitHub. Нашла несколько вариантов, но ни один из них не имеет отдельного backend и frontend (ну или я не поняла, что в целом там было написано). В общем просто скопировать код не получится и придется помучиться. Далее начинаю думать: какие инструменты я вообще знаю. На моем компьютере установлена IDE PyCharm. В условии написано, что backend можно сделать на Python. Это отличная мысль, осталось понять как сделать backend, который раньше никто из нашей команды не делал. Тем не мее, визуально понравился [этот вариант](https://clow-note.vercel.app/) 

Пошла в интернет, нашла несколько статей на хабре. Вот [эта статья](https://habr.com/ru/companies/otus/articles/886390/?ysclid=mu2b9dozsv310120921) стала отправной точкой для реализации Backend'а на Python с использованием Flask. Всё бы ничего, но этой статьи оказалось недостаточно. ВсЁ, я окончательно запуталась и силы начали меня покидать.

Пришло время обратиться к ИИ-шке. К сожалению, информации вылилось столько, что голова разорвалась окончательно. Какие-то миграции и модели, что всё это значит? ВсЁ, нам точно крышка, пронеслось в голове...

Берем себя в руки. 
Начинаем делать проект в PyCharm, чтобы реализовать простейший backend. Разработку ведем в ОС Windows 10. В качестве терминала используем Git Bash, чтобы получить совместимость с консолью ОС Linux (скорее всего пригодится на будущее).

<details>
<summary>Создаем такую структуру проекта и наполняем кодом:</summary>

```text
webnotes_service/
├── backend/
│   ├── .env                     
│   ├── run.py
│   ├── app/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   ├── extensions.py
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   └── notes.py
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   └── note.py
│   │   └── services/
│   │       └── note_service.py
│   └── migrations/
│
├── frontend/
└── README.md
```
</details>

Создаем виртуальное окружение Python (здесь и далее все команды будем выполнять в терминале Git Bash на машине с OC Windows 10) и активируем его:
```bash
cd webnotes_service/backend
python -m venv .venv
source .venv/Scripts/activate
```
В активированное виртуальное окружение ставим нужные для backend'a пакеты:
```bash
python -m pip install \
  Flask \
  Flask-Cors \
  Flask-Migrate \
  Flask-SQLAlchemy \
  "psycopg[binary]" \
  python-dotenv
```

Чтобы восстановить пакеты, на другой машине запоминаем все что установили в файле requirements.txt:

```bash
python -m pip freeze > requirements.txt
```
После выполнения этой команды файл requirements.txt будет помещен в корень каталога backend. 

Пробуем запустить backend:
```bash
flask --app run.py run --debug --port 5100
```
В терминале видим такой вывод:
```text
 * Serving Flask app 'run.py'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5100
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 986-046-009
```
значит backend запустился и слушает на порту 5100.

Пробуем вызвать эндпоинт health:
```bash
curl "http://127.0.0.1:5100/health"
```
В терминале видим такой вывод:
```text
{
  "status": "ok"
}
```
Ура, это маленькая победа. Теперь нужно решить, какие эндпоинты будем реализовывать и как заставить работать backend с базой данных. Но это уже не сегодня, т.к. уже глубокая ночь и скорее всего уже наступило завтра. Счет времени потерян окончательно... 

## День 3-й. Frontend
Попросили ИИ-шку сделать страницу заметок [в этом стиле](https://clow-note.vercel.app/) и доработать код backend'a. 

В результате получаем перечень эндпоинтов для backend'a, огромное количество непонятного кода и эту команду для создания проекта frontend через React и Vite:

```bash
cd webnotes_service

npm create vite@latest frontend -- \
  --template react-ts

```
В результате получили:
```text
$ npm create vite@latest frontend -- \
  --template react-ts
bash: npm: command not found
```
Поисковик дал ответ:
```text
Node.js не установлен. npm поставляется вместе с Node.js, поэтому без установки Node.js npm тоже не будет работать.
```
Ставим Node.js, скачав его [с официального сайта](https://nodejs.org/en/download). 

<details>
  <summary>Теперь проект через npm создается и запускается.</summary>

```text
$ npm create vite@latest frontend -- \
  --template react-ts
Need to install the following packages:
create-vite@9.2.1
Ok to proceed? (y) y

> npx
> create-vite frontend --template react-ts

│
◇  Which linter to use?
│  Oxlint
│
◇  Install with npm and start now?
│  Yes
│
◇  Scaffolding project in  C:\Users\Lisa\work\DevOps_Lab\webnotes_service\frontend...
│
◇  Installing dependencies with npm...

added 27 packages, and audited 28 packages in 16s

9 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
│
◇  Starting dev server...

> frontend@0.0.0 dev
> vite


  VITE v8.3.0  ready in 447 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```
</details>

<details>
  <summary>Открываем в браузере URL: http://localhost:5173/ и видим стартовую страницу проекта.</summary>

![img.png](Screenshots/vite_start_page.png)

</details>

<details>
<summary>Дорабатываем созданный проект до такой структуры и наполняем кодом.</summary>

```text
webnotes_service/
├── backend/
├── frontend/src/
│   ├── api/
│   │   └── client.ts
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   ├── NotesList.tsx
│   │   ├── NoteCard.tsx
│   │   ├── NoteEditor.tsx
│   │   ├── PhotoStack.tsx
│   │   └── EmptyState.tsx
│   ├── layouts/
│   │   └── AppLayout.tsx
│   ├── types/
│   │   └── note.ts
│   ├── services/
│   │   └── note_service.py
│   ├── App.css
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
└── README.md
```
</details>

Пробуем запускать наш frontend:
```bash
npm run dev
```

<details>
  <summary>И в браузере по url http://localhost:5173/ видим страницу такую красоту.</summary>

![img.png](Screenshots/notes_page.png)

</details>

Теперь наш frontend полностью готов. Нужно доработать backend и разобраться с базой данных. 

## День 4-й. Backend, миграции и база данных
### Backend
Окончательный перечень эндпоинтов для backend'a выглядит так: 
```text
GET    /health
GET    /api/notes
GET    /api/notes/<id>
POST   /api/notes
PATCH  /api/notes/<id>
DELETE /api/notes/<id>
```
### База данных
Базу данных будем использовать [PostgreSQL](https://www.postgresql.org/). Для Windows 10 [cкачиваем установщик](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads) и устанавливаем СУБД PostgreSQL. 

<details>
  <summary>В процессе установки выбираем компоненты: PosgteSQL Server, pgAdmin 4, Command Line Tools. На всякий случай выбрали установку всех компонент.</summary>

![img.png](Screenshots/pg_installer.png)

</details>

Также, в процессе установки PostgreSQL вводим и запоминаем пароль суперпользователя postgres.  
После установки PostgreSQL нам становится доступна графическая утилита для администрирования PgAdmin 4. 
 
<details>
  <summary>Запускаем PgAdmin 4, вводим пароль суперпользователя postgres и входим на наш локальный PosgteSQL Server.</summary>

![img.png](Screenshots/PgAdmin_1.png)

![img.png](Screenshots/PgAdmin_2.png)

</details>

Теперь, используя PgAdmin 4 создадим пустую базу данных (БД), к которой будет подключаться наш backend. Имя БД указываем в строке подключения к БД в файле ./backend/.env.
Строка подключения имеет вид:

```ini
DATABASE_URL=postgresql+psycopg://username:passwor@127.0.0.1:5432/notes_service
```
<details>
  <summary>Создаем пустую БД с именем notes_service. </summary>

![img.png](Screenshots/PgAdmin_createDB.png)
![img.png](Screenshots/PgAdmin_createDB_2.png)

</details>

### Миграции
Теперь нам нужно создать структуру данных в нашей БД. Это делается при помощи механизма под названием "Миграции".

```bash
flask --app run.py db migrate -m "create notes table"
```
Команда db migrate только записывает найденные изменения в файл миграции. Она не изменяет базу. Для применения нужно выполнить:

```bash
flask --app run.py db upgrade
```
После чего в нашей БД notes_service будет создана таблица с именем notes.

### Первый запуск нашего сервиса
Теперь с замиранием сердца пробуем запустить наш сервис.

Запускаем backend (в первом окне терминала Git Bash):

```bash
cd webnotes_service/backend/
flask --app run.py run --debug --port 5100
```

Затем запускаем frontend (во втором окне терминала Git Bash):

```bash
cd webnotes_service/frontend/
npm run dev
```
Ура!!! Теперь сервис Заметок полностью работает. Теперь не стыдно выгрузить в [репозиторий на GitHub](https://github.com/ZhukovaE/Lab0.git). 

## День 5-й. Попытка запустить сервис Заметок вручную после клонирования репозитория
Теперь пробуем запустить наш сервис Заметок. 