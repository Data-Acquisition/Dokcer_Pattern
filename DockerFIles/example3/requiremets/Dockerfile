# Используем официальный образ Python
FROM python:3.8

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /usr/src/app

# Копируем зависимости
COPY requirements.txt ./

# Устанавливаем зависимости
RUN pip install --no-cache-dir -r requirements.txt

# Копируем исходный код
COPY .. .

# Команда для запуска консольного приложения
CMD ["python", "console_app.py"]
