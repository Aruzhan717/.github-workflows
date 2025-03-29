name: Deploy Flask App

on:
  push:
    branches:
      - main  # Этот workflow будет запускаться при пуше в ветку main

jobs:
  build:
    runs-on: ubuntu-latest  # Запуск на последней версии Ubuntu

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2  # Клонируем репозиторий

      - name: Set up Python
        uses: actions/setup-python@v2  # Настроим Python

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt  # Устанавливаем зависимости

      - name: Build Docker image
        run: |
          docker build -t flask-app .  # Собираем Docker-образ

      - name: Run Docker container
        run: |
          docker run -d -p 5000:5000 flask-app  # Запускаем контейнер

      - name: Verify the app is running
        run: |
          curl -f http://localhost:5000  # Проверяем, что приложение работает
