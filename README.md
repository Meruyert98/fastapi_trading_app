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

Gmail - add 2authentication - app passwords
In .env file change smtp_user and smtp_pass

Testing the app
`pytest test/`

CORS - набор HTTP заголовков, который позвлояют общаться друг с другом.

Middleware - среднее звено, которое обрабатывает запрос с фронта на бэкенд.

- yield - он отдает нам контроль на некоторое время. И как только мы отдавали ответ пользователю, сессия уничтажалась.Здесь использование Depends для каких-то временных соединений.

```python
from fast
async def get_async_session():
    print("Получение сессии")
    session = "session"
    yield session
    print("Уничтожение сессии")


@app.get("/items")
async def get_items(session=Depends(get_async_session)):
    return [{"id": 1}]
```

- parameters - параметры зависят от выводящий данных Depends функций

```python

def pagination_params(limit: int = 10, skip: int = 0):
    return {"limit": limit, "skip":skip}

@app.get("/subjects")
async def get_subjects(pagination_params: dict = Depends(pagination_params)):
    return pagination_params
```

- class - параметры зависят от выводящий данных Depends class

```python

class Paginator:
    def __init__(self, limit: int = 10, skip: int = 0):
        self.limit = limit
        self.skip = skip

@app.get("/subjects_class")
async def get_subjects_class(pagination_params: Paginator = Depends(Paginator)):
    return pagination_params
```

- dependencies - у нас есть ендпойнт который имеет какую-то зависимость, фастапи перед вызовом фцнкций прогонит все зависимости, чтобы убедиться что они вернулись без ошибки.

```python
class AuthGuard:
    def __init__(self, name: str):
        self.name = name

    def __call__(self, request: Request):
        if "super_cookie" not in request.cookies:
            raise HTTPException(status_code=403, detail="Запрещено")
        # проверяем что в куках есть инфа о наличии прав пользователя
        return True

auth_guard_payments = AuthGuard("payments")

# first version
@app.get("/payments")
def get_payments(auth_guard_payments: AuthGuard = Depends(auth_guard_payments)):
    return "my payments ..."

# second version
@app.get("/payments", dependencies=[Depends(auth_guard_payments)])
def get_payments():
    return "my payments ..."

# third version, look for all router
router = APIRouter(
    dependencies=[Depends(auth_guard_payments)]
)
```

`docker build . -t fastapi_app:latest`

`docker run -d -p 7330:8000 fastapi_app`
