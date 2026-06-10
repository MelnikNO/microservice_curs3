# microservice_curs3

# Микросервисная архитектура Telegram-бота и веб-лендинга

[![Telegram Bot](https://img.shields.io/badge/Telegram-@galaxy_milk_bot-blue.svg)](https://t.me/galaxy_milk_bot)
[![Website](https://img.shields.io/badge/Website-vebfortgbot.melnikno.bizml.ru-green.svg)](https://vebfortgbot.melnikno.bizml.ru/)
[![Docker](https://img.shields.io/badge/Docker-✓-2496ED.svg)](https://www.docker.com/)

**Telegram Бот "Галактика"** — бот-визитка для компании «Галактика» (производитель молочной продукции).

## О проекте

Проект демонстрирует **микросервисную архитектуру**, состоящую из двух независимых сервисов:

| Сервис | Описание |
|--------|----------|
| **Telegram-бот** | Предоставляет информацию о компании, брендах, продукции, новостях и контактах |
| **Веб-лендинг** | Презентационная страница с 6 карточками-ссылками на бота, защищён HTTPS |

## Технологии

- Python 3.12 + python-telegram-bot
- HTML5 + CSS3 (адаптивный дизайн)
- Docker + Docker Compose
- NGINX (nginx-proxy) + acme-companion (Let's Encrypt)

## Предварительные требования

Для запуска проекта вам понадобятся:

| Компонент | Версия | Проверка |
|-----------|--------|----------|
| Python | 3.10+ | `python --version` |
| Docker | 20.10+ | `docker --version` |
| Docker Compose | 2.0+ | `docker-compose --version` |
| Git | любая | `git --version` |

## Запуск проекта

### 1. Клонирование репозитория

```bash
git clone https://github.com/MelnikNO/microservice_curs3.git
cd galaxy-bot
```

### 2. Запуск Telegram-бота (локально)

Создайте виртуальное окружение

```
python -m venv venv
source venv/bin/activate  # Linux/Mac
# или
venv\Scripts\activate     # Windows
```

Установите зависимости

```pip install -r bot/requirements.txt```

Создайте файл .env с токеном бота

```echo "BOT_TOKEN=ваш_токен_от_BotFather" > .env```

Запустите бота

```python bot/bot.py```

### 3. Запуск веб-лендинга (локально, без HTTPS)

Перейдите в папку с лендингом

```cd website```

Запустите простой HTTP-сервер

```python -m http.server 8000```

**Проверка:** Откройте браузер → http://localhost:8000 — должен открыться лендинг с 6 карточками.

### 4. Запуск веб-лендинга в Docker (локально)

Вернитесь в корень проекта

```cd ..```

Запустите контейнер

```docker-compose up -d```

**Проверка:** Откройте браузер → http://localhost — должен открыться лендинг.

## Развёртывание на сервере

Предварительные требования для сервера:

* Сервер (VPS) с публичным IP

* Домен, настроенный на IP сервера (A-запись)

* Открытые порты 80 и 443

### Шаг 1: Установка Docker на сервере
```
ssh root@ваш-ip-сервера
apt update && apt upgrade -y
apt install docker.io docker-compose -y
systemctl enable docker
systemctl start docker
```

### Шаг 2: Копирование проекта на сервер
```
# На вашем компьютере
scp -r microservice_curs3 root@ваш-ip-сервера:/root/
```

### Шаг 3: Настройка переменных окружения
```
# На сервере
cd /root/galaxy-bot
cp .env.example .env
nano .env
```

Содержимое .env:

```
DOMAIN_NAME=ваш-домен.ru
CERTBOT_EMAIL=ваш-email@example.com
```

### Шаг 4: Запуск веб-лендинга с HTTPS
```
# Создайте папки для nginx-proxy
mkdir -p nginx/{conf.d,vhost.d,html,certs}

# Запустите контейнеры
docker-compose up -d
```

**Проверка:** Откройте браузер → https://ваш-домен.ru — должен открыться лендинг
