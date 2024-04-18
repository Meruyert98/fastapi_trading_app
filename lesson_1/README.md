## Установка и запуск

Создаем виртуальное окружение

```
python -m venv venv
```

Активация виртуального окружения (ubuntu)

```
source venv/bin/activate
```

Активация виртуального окружения (Windows)

```
venv\Scripts\activate.bat
```

Установка fastapi

```
pip install fastapi[all]
```

Запуск приложения и автоматически перезапуск при изменении кода (увидеть изменения в режиме live)

```
uvicorn main:app --reload
```
