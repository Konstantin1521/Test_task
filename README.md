# FastAPI Image Management Application

Это асинхронное приложение на FastAPI, использующее RabbitMQ для обработки сообщений и PostgreSQL в качестве базы данных. Приложение поддерживает аутентификацию с помощью JWT токенов.

## Описание проекта

Проект представляет собой RESTful API, который позволяет создавать, читать, обновлять и удалять изображения. API использует RabbitMQ для обработки сообщений и PostgreSQL в качестве базы данных.

## Структура проекта

.<br>
├── main.py # Основной код приложения<br>
├── api/ # Определение маршрутов API<br>
├── auth/ # Аутентификация<br>
├── certs/ # Место хранения публичного и приватного ключей<br>
├── core/ # Конфигурации настройки приложения и модели базы данных<br>
├── rabbitmq/ # Логика работы с RabbitMQ<br>
├── alembic/ # Миграции базы данных Alembic<br>
│ └── versions/ # Версии миграций<br>
├── Dockerfile # Конфигурация Docker для приложения<br>
└── docker-compose.yml # Конфигурация Docker Compose для запуска приложения<br>

# Подготовка к запуску

Для запуска проекта необходимо выполнить следующие действия:

## 1.Создание виртуального хоста

Если виртуальный хост еще не создан, необходимо создать его вручную:

1. Подключитесь к RabbitMQ Management UI (доступен по адресу `http://localhost:15672`).
2. Войдите с использованием учетных данных.
3. Перейдите на вкладку "Admin".
4. В разделе "Virtual Hosts" нажмите "Add a new virtual host".
5. Введите имя виртуального хоста и нажмите "Add virtual host".

##  Доступ к RabbitMQ

После запуска приложения необходимо зайти в RabbitMQ.

1. Подключитесь к RabbitMQ Management UI (доступен по адресу `http://localhost:15672`).
2. Вйдите с использованием учетных данных которые вы указали в файле `docker-compose.yml`.


## 2.Создание базы данных

Для создания таблиц в базе данных необходимо выполнить следующую команду из папки fastapi-application:

```bash
alembic upgrade head
```
миграция уже находится в папке alembic/versions

## 3. Генерация токенов

Для генерации токенов необходимо выполнить следующую команду из папки fastapi-application:

```shell
# Генерация RSA приватного ключа
openssl genrsa -out jwt-private.pem 2048
```

```shell
# Генерация RSA публичного ключа
openssl rsa -in jwt-private.pem -outform PEM -pubout -out jwt-public.pem
```

ключи должны находится в папке fastapi-application/certs

## Запуск приложения

Для запуска приложения используйте Docker Compose. Перейдите в папку с проектом и выполните следующую команду:

```bash
docker-compose up --build
```

## Примеры использования API

- GET запросы
Получить список всех изображений:

GET http://0.0.0.0:8000/api/v1/get_all

Получить изображение по ID:
GET http://0.0.0.0:8000/api/v1/get/<id>

- POST запросы 
Создать новое изображение:
POST http://0.0.0.0:8000/api/v1/create/

Требуется передать в теле запроса:

json
{
  "title": "string",
  "file_path": "string",
  "resolution": "string",
  "size": 0
}

- PUT запрос
Обновить изображение:

PUT http://0.0.0.0:8000/api/v1/update/<id>
Требуется передать в теле запроса:

json
{
  "title": "new title",
  "file_path": "home",
  "resolution": "800x600",
  "size": 1024
}


DELETE запрос

Удалить изображение:
DELETE http://0.0.0.0:8000/api/v1/delete/<id>


- Аутентификация

Создать пользователя:
POST http://0.0.0.0:8000/api/v1/register
Требуется передать логин и пароль.

Выпуск токена:
POST http://0.0.0.0:8000/api/v1/auth/login
Требуется передать логин и пароль.

