# Используем официальный образ Python
FROM python:3.8

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /usr/src/app

# Копируем файлы зависимостей и Poetry.lock
COPY pyproject.toml poetry.lock ./

# Устанавливаем Poetry
RUN pip install poetry

# Устанавливаем зависимости
RUN poetry install

# Копируем исходный код
COPY . .

# Команда для запуска консольного приложения
CMD ["poetry", "run", "python", "console_app.py"]
