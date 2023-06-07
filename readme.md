# Установка Docker, Docker Compose и PostgreSQL на Windows

Это руководство позволит вам установить Docker, Docker Compose и PostgreSQL на операционной системе Windows с использованием Docker Compose.

## Шаг 1: Установка Docker

1. Перейдите на официальный сайт Docker (<https://www.docker.com/>) и скачайте установочный файл Docker Desktop для Windows.
2. Запустите установщик и следуйте инструкциям на экране для установки Docker.
3. После завершения установки запустите Docker Desktop.

## Шаг 2: Проверка установки Docker

1. Откройте командную строку (Command Prompt) или PowerShell. (wir+r) введите команду `cmd` и нажмите Enter.
2. Введите команду `docker version` и нажмите Enter.
3. Если установка прошла успешно, вы должны увидеть информацию о версии Docker Engine и Docker Compose.

## Шаг 3: Создание файла docker-compose.yml для создания контейнера PosrgreSQL

1. Создайте новую пустую папку, где будет находиться ваш проект.
2. В этой папке создайте новый текстовый файл и назовите его `docker-compose.yml`.
3. Откройте `docker-compose.yml` в текстовом редакторе и добавьте следующий код:

```yaml
version: '3'
services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_PASSWORD: your_postgres_password
      POSTGRES_USER: your_postgres_username
    volumes:
      - ./data:/var/lib/postgresql/data
    ports:
      - 5432:5432
```

- опция для автоматического удаления контейнера после его остановки (если требуется), нужно добавить в `docker-compose.yaml`

```yaml
options:
      --rm
```

- `version: '3'`: Указывает версию синтаксиса Docker Compose, которую мы используем. В данном случае, это версия 3.

- `services`: Определяет список сервисов, которые будут запущены с помощью Docker Compose.

- `db`: Название нашего сервиса, в данном случае, это PostgreSQL.

- `image: postgres`: Указывает Docker, чтобы он загрузил образ PostgreSQL из Docker Hub.

- `restart: always`: Указывает Docker перезапустить контейнер автоматически, если он неожиданно остановится.

- `environment`: Определяет переменные окружения, которые будут переданы в контейнер PostgreSQL. Здесь мы устанавливаем пароль и имя пользователя для доступа к базе данных.

- `volumes`: Монтирует локальную папку ./data внутрь контейнера PostgreSQL для хранения данных базы данных.

  `./data:/var/lib/postgresql/data:`

  `./data` - это путь на хост-машине (локальной системе), где будет располагаться директория данных для контейнера.
`/var/lib/postgresql/data` - это путь внутри контейнера, где PostgreSQL ожидает наличие данных.
Это опция связывает директорию на хост-системе с директорией данных PostgreSQL внутри контейнера. Все данные, созданные или измененные в контейнере, будут сохраняться в этой директории. При следующем запуске контейнера эти данные будут доступны внутри контейнера.

  `./init.sql:/docker-entrypoint-initdb.d/init.sql:`

  `./init.sql` - это путь к файлу init.sql, содержащему SQL-скрипт инициализации базы данных.

  `/docker-entrypoint-initdb.d/init.sql` - это путь внутри контейнера, где входная точка инициализации PostgreSQL ожидает наличие SQL-скриптов.

- `ports`: Пробрасывает порт 5432 на локальной машине на порт 5432 внутри контейнера, чтобы мы могли подключаться к базе данных с внешнего устройства.

Обратите внимание, что вам необходимо заменить `your_postgres_password` на фактический пароль, который вы хотите использовать для пользователя PostgreSQL, и `your_postgres_username` на фактическое имя пользователя, которое вы хотите использовать.

## Шаг 4: Запуск контейнера PostgreSQL с помощью Docker Compose

1. Откройте командную строку (Command Prompt) или PowerShell. (wir+r) введите команду `cmd` и нажмите Enter.
2. Перейдите в папку, где находится ваш файл `docker-compose.yml` с помощью команды `cd <путь до папки с файлом>`.
3. Введите команду `docker-compose up -d` и нажмите Enter.
4. Docker Compose загрузит образ PostgreSQL, создаст и запустит контейнер.
5. Когда контейнер будет успешно запущен, вы сможете подключиться к базе данных PostgreSQL с помощью выбранного вами инструмента.

## Шаг 4: Проверка установки и запуска контейнера PostgreSQL

1. Чтобы проверить запустился ли контейнер с PostgreSQL, откройте командную строку (Command Prompt) или PowerShell. (wir+r) введите команду `cmd` и нажмите Enter.
2. Введите команду `docker ps`.
Если все успешно установилось и запустилось, вы увидите в списоке контейнер postgres.

```bash
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                    NAMES
04ff43fe51f3   postgres   "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   0.0.0.0:5432->5432/tcp   docker-db-1
```

3. Если контейнер автоматически не запустился можно проверить командой `docker ps -a` создался контейнер.

```bash
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS                          PORTS     NAMES
04ff43fe51f3   postgres   "docker-entrypoint.s…"   26 minutes ago   Exited (0) About a minute ago             docker-db-1
```

Для запуска используйте команду (если не настроен автоматических запуск контейнера при создании) `docker start <имя контейнера или его ID>`

Так же проверить корректность установки, запуск, управление контейнером можно в Docker Desktop.

Теперь вы установили Docker, Docker Compose и запустили контейнер PostgreSQL с помощью Docker Compose.

## Шаг 5: Подключение к базе PostgresSQL

Вы можете подключиться к базе данных с использованием выбранного вами инструмента для управления PostgreSQL. Вот как это можно сделать:

1. Откройте инструмент для управления базами данных PostgreSQL, например, pgAdmin, DBeaver или командную строку psql.

2. Введите следующие данные для подключения:

- `Хост`: localhost или 127.0.0.1 (так как мы пробросили порт 5432 в контейнере на порт 5432 на локальной машине)

- `Порт`: 5432 (порт, который мы указали в docker-compose.yml)

- `Имя пользователя`: ваше выбранное имя пользователя (то, что вы указали в `docker-compose.yml` в поле `POSTGRES_USER`)

- `Пароль`: ваш выбранный пароль (то, что вы указали в `docker-compose.yml` в поле `POSTGRES_PASSWORD`)

- `База данных`: по умолчанию PostgreSQL создает базу данных с именем, совпадающим с именем пользователя, которое вы указали при установке (можно указать другое имя базы данных, если требуется)

- команда для подключения к контейнеру с базой через командную строку и вызова psql

```bash
docker exec -it my_db_container psql -U root -d root
```

- команда psql для добавления новых данных в базу

```bash
INSERT INTO public.index_mass (user_id, weight, height) VALUES (4, 80, 178);
```

3. Нажмите на кнопку "Подключиться" или выполните соответствующую команду для подключения к базе данных.

После успешного подключения вы сможете работать с базой данных PostgreSQL через выбранный вами инструмент для управления базами данных.
