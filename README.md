# Trading App

Создать окружение
`python -m venv venv`

Зайти на окружение
`.\venv\Scripts\activate`

Скачать все библиотеки fastapi
`pip install fastapi[all]`

Запустить приложение
`uvicorn main:app --reload`

`alembic init migrations`

Сделать ревизию для миграцию в БД
`alembic revision --autogenerate -m "Database creation"`

Запустить миграцию
`alembic upgrade 25075e23b9b2` - хэш ревизии

`alembic upgrade head`

`pip install fastapi-users[sqlalchemy]`

`pip install asyncpg`

Запустить celery
`celery -A src.tasks.tasks:celery worker --loglevel=INFO --pool=solo`

Запустить flower
`celery -A src.tasks.tasks:celery flower`

Flower web localhost:5555
