<div align="center">

# FashionFlow

**Современная fashion e-commerce платформа с виртуальной примеркой, AI-рекомендациями образов, стилевыми профилями и полным циклом заказов — ваш собственный Zalando / ASOS под полным контролем.**

<br>

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Django](https://img.shields.io/badge/Django-5.1-092E20?style=for-the-badge&logo=django&logoColor=white)](https://www.djangoproject.com/)
[![DRF](https://img.shields.io/badge/DRF-3.15-092E20?style=for-the-badge&logo=django&logoColor=white)](https://www.django-rest-framework.org/)
[![Vue.js](https://img.shields.io/badge/Vue.js-3.5-4FC08D?style=for-the-badge&logo=vuedotjs&logoColor=white)](https://vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-6-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)
[![Celery](https://img.shields.io/badge/Celery-5.4-37814A?style=for-the-badge&logo=celery&logoColor=white)](https://docs.celeryq.dev/)
[![Docker](https://img.shields.io/badge/Docker_Compose-3.9-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docs.docker.com/compose/)
[![Nginx](https://img.shields.io/badge/Nginx-1.25-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.org/)
[![Stripe](https://img.shields.io/badge/Stripe-API-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://stripe.com/)
[![License](https://img.shields.io/badge/License-Proprietary-555555?style=for-the-badge)](LICENSE)

</div>

---

## Содержание

1. [О проекте](#1-о-проекте)
2. [Ключевые возможности](#2-ключевые-возможности)
3. [Технологический стек](#3-технологический-стек)
4. [Структура репозитория](#4-структура-репозитория)
5. [Архитектура и как это работает](#5-архитектура-и-как-это-работает)
6. [Доменная модель (крупными блоками)](#6-доменная-модель-крупными-блоками)
7. [Сервисы в Docker Compose](#7-сервисы-в-docker-compose)
8. [Быстрый старт (локально, Docker)](#8-быстрый-старт-локально-docker)
9. [Основные команды](#9-основные-команды)
10. [Ручной запуск frontend и backend](#10-ручной-запуск-frontend-и-backend)
11. [Конфигурация и переменные окружения](#11-конфигурация-и-переменные-окружения)
12. [API, очереди и интеграции](#12-api-очереди-и-интеграции)
13. [Мониторинг и эксплуатация](#13-мониторинг-и-эксплуатация)
14. [CI/CD](#14-cicd)
15. [Безопасность и хранение медиа](#15-безопасность-и-хранение-медиа)
16. [Роли компонентов в продакшене](#16-роли-компонентов-в-продакшене)
17. [Лицензия](#17-лицензия)
18. [Поддержка](#18-поддержка)

---

## 1. О проекте

**FashionFlow** — продуктовая fashion e-commerce платформа уровня production: каталог одежды и аксессуаров, персонализация стиля, конструктор образов, отзывы с фото, промо-механики и полный жизненный цикл заказа — от корзины до возврата и возмещения.

Платформа рассчитана на три аудитории:

| Аудитория | Интерфейс | Задачи |
|-----------|-----------|--------|
| **Покупатели** | Vue.js SPA | Каталог, примерка, образы, заказы, отзывы |
| **Мерчанты / админы** | Django Admin | Каталог, склад, промо, заказы, аналитика |
| **Интеграторы** | REST API + OpenAPI | Внешние витрины, мобильные приложения, партнёры |

### Что это за тип системы

FashionFlow — **распределённая мультисервисная платформа**, а не монолитный скрипт. Бизнес-логика сосредоточена в Django API, фоновые процессы вынесены в Celery, клиентский опыт — в отдельном SPA, а единая точка входа обеспечивается Nginx.

| Аспект | Описание |
|--------|----------|
| **Продукт** | B2C fashion e-commerce с элементами персонализации и UGC (отзывы, доски стиля) |
| **Архитектура** | Django REST API · Vue 3 SPA · Celery Worker + Beat · Nginx reverse proxy |
| **Хранилище** | PostgreSQL 16 (транзакционные данные) · Redis 7 (кэш, сессии, брокер) · S3 / локальные тома (медиа) |
| **Платежи** | Stripe (карты, webhooks) |
| **Документация API** | drf-spectacular → Swagger UI на `/api/docs/` |

---

## 2. Ключевые возможности

### Для покупателей

| Возможность | Описание |
|-------------|----------|
| **Каталог товаров** | Фильтрация по категории, коллекции, сезону, бренду, цвету и размеру |
| **Виртуальная примерка** | Загрузка фото и визуализация вещей на себе |
| **Конструктор образов** | Сборка комплектов из нескольких товаров |
| **Стилевой профиль** | Анкета стиля для персонализированных рекомендаций |
| **Доски стиля** | Pinterest-подобные доски для вдохновения и шаринга |
| **Гид по размерам** | Рекомендации размера по замерам (грудь, талия, бёдра, EU/UK/US) |
| **Сезонные коллекции** | Кураторские подборки по сезону и трендам |
| **Отзывы и рейтинги** | Фото-отзывы с оценкой посадки (маломерит / в размер / большемерит) |
| **Промоакции** | Купоны, flash-sale, процентные и фиксированные скидки |
| **Заказы** | Полный цикл: корзина → оплата → доставка → возврат / refund |

### Для бизнеса и администрирования

| Возможность | Описание |
|-------------|----------|
| **Партнёрства с инфлюенсерами** | Промокоды, комиссии, отслеживание эффективности |
| **Управление складом** | Остатки по вариантам (SKU), алерты низкого остатка |
| **Промо-движок** | Создание и планирование кампаний, купонов, распродаж |
| **Django Admin** | Полноценная панель управления всеми сущностями |

---

## 3. Технологический стек

### Backend

| Компонент | Технология | Назначение |
|-----------|------------|------------|
| Runtime | Python 3.12+ | Серверная логика |
| Framework | Django 5.1 | ORM, admin, безопасность |
| API | Django REST Framework 3.15 | REST endpoints, сериализация |
| Auth | SimpleJWT 5.4 | JWT access/refresh, blacklist |
| Фильтрация | django-filter 24.3 | Query-параметры каталога |
| Категории | django-mptt 0.16 | Иерархическое дерево категорий |
| Документация | drf-spectacular 0.28 | OpenAPI 3 + Swagger UI |
| Задачи | Celery 5.4 + django-celery-beat | Email, напоминания, расписание |
| Кэш | django-redis 5.4 | Сессии и кэш приложения |
| Медиа | Pillow + django-storages + boto3 | Изображения, S3 |
| Платежи | stripe 11.4 | Checkout и webhooks |
| WSGI | Gunicorn 23 | Production HTTP-сервер |

### Frontend

| Компонент | Технология | Назначение |
|-----------|------------|------------|
| Framework | Vue.js 3.5 (Composition API) | SPA-интерфейс |
| Сборка | Vite 6 | Dev-сервер и production build |
| Маршрутизация | Vue Router 4.5 | Клиентские маршруты |
| Состояние | Vuex 4 + Pinia 2 | Глобальный store |
| HTTP | Axios 1.7 | API-клиент |
| Утилиты | @vueuse/core 12 | Composition-хелперы |

### Инфраструктура

| Компонент | Версия | Роль |
|-----------|--------|------|
| PostgreSQL | 16 Alpine | Основная БД |
| Redis | 7 Alpine | Кэш, брокер Celery, сессии |
| Nginx | 1.25 Alpine | Reverse proxy, static/media |
| Docker Compose | 3.9 | Оркестрация локально и в prod |

---

## 4. Структура репозитория

```
FashionFlow/
├── backend/                          # Django-приложение
│   ├── apps/
│   │   ├── accounts/                 # Пользователи, стилевые профили, адреса
│   │   ├── products/                 # Каталог, категории, коллекции, варианты
│   │   ├── orders/                   # Заказы, возвраты, refunds, Celery-задачи
│   │   ├── outfits/                  # Образы, доски стиля
│   │   ├── reviews/                  # Отзывы, фото, голоса «полезно»
│   │   └── promotions/               # Купоны, flash-sale, промо-кампании
│   ├── config/
│   │   ├── settings/                 # base · development · production
│   │   ├── urls.py                   # Маршрутизация API и admin
│   │   ├── wsgi.py
│   │   └── celery.py                 # Celery app
│   ├── utils/                        # Пагинация, обработчик исключений
│   ├── manage.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/                         # Vue 3 SPA
│   ├── src/
│   │   ├── api/                      # Axios client, endpoints
│   │   ├── components/               # UI-компоненты (auth, cart, layout…)
│   │   ├── views/                    # Страницы (Dashboard, Settings…)
│   │   ├── router/                   # Vue Router
│   │   ├── store/                    # Vuex / Pinia modules
│   │   ├── App.vue
│   │   └── main.js
│   ├── public/
│   ├── vite.config.js
│   ├── package.json
│   └── Dockerfile
├── nginx/
│   └── nginx.conf                    # Проксирование API, static, SPA
├── docker-compose.yml                # Полный стек сервисов
├── .env.example                      # Шаблон переменных окружения
├── .gitignore
└── README.md
```

---

## 5. Архитектура и как это работает

```mermaid
flowchart TB
    subgraph Client["Клиент"]
        Browser["Браузер / мобильное приложение"]
    end

    subgraph Edge["Edge"]
        Nginx["Nginx :80 / :443"]
    end

    subgraph App["Application Layer"]
        FE["Vue 3 SPA\n(Vite build)"]
        API["Django + DRF\nGunicorn :8000"]
        CW["Celery Worker"]
        CB["Celery Beat"]
    end

    subgraph Data["Data Layer"]
        PG[("PostgreSQL 16")]
        RD[("Redis 7")]
        S3[("S3 / Media Volume")]
    end

    subgraph External["Внешние сервисы"]
        Stripe["Stripe"]
        SMTP["SMTP"]
        Sentry["Sentry"]
    end

    Browser --> Nginx
    Nginx -->|"/"| FE
    Nginx -->|"/api/ · /admin/"| API
    Nginx -->|"/static/ · /media/"| S3
    API --> PG
    API --> RD
    API --> S3
    CW --> PG
    CW --> RD
    CB --> RD
    API --> Stripe
    CW --> SMTP
    API --> Sentry
```

**Типовой сценарий заказа:**

1. Покупатель добавляет варианты товара (цвет + размер) в корзину через SPA.
2. SPA отправляет `POST /api/orders/` с JWT-токеном.
3. Django создаёт заказ, резервирует остатки, инициирует оплату через Stripe.
4. Celery отправляет письмо подтверждения (`send_order_confirmation_email`).
5. При смене статуса на «shipped» — уведомление с трек-номером (`send_shipping_notification`).
6. Beat по расписанию запускает напоминания о брошенной корзине (`send_abandoned_cart_reminders`).

---

## 6. Доменная модель (крупными блоками)

### `accounts` — пользователи и персонализация

| Сущность | Назначение |
|----------|------------|
| `User` | Кастомная модель пользователя (email, профиль) |
| `StyleProfile` | Стилевые предпочтения для рекомендаций |
| `Address` | Адреса доставки и выставления счёта |

### `products` — каталог

| Сущность | Назначение |
|----------|------------|
| `Category` | Иерархия категорий (MPTT-дерево) |
| `Product` | Карточка товара (бренд, сезон, описание) |
| `ProductVariant` | SKU: цвет + размер + цена + остаток |
| `ProductImage` | Галерея изображений |
| `Collection` | Сезонные / тематические подборки |
| `Color`, `Size` | Справочники с замерами и EU/UK/US-эквивалентами |

### `orders` — коммерция

| Сущность | Назначение |
|----------|------------|
| `Order` | Заказ со снимком адреса и итогами |
| `OrderItem` | Позиции заказа (денормализованные названия) |
| `Return` | Заявка на возврат |
| `Refund` | Возмещение средств |

### `outfits` — стиль и UGC

| Сущность | Назначение |
|----------|------------|
| `Outfit` | Собранный образ из нескольких товаров |
| `OutfitItem` | Связь образ ↔ товар |
| `StyleBoard` | Доска вдохновения (публичная / приватная) |

### `reviews` — доверие

| Сущность | Назначение |
|----------|------------|
| `Review` | Текст, рейтинг, оценка посадки |
| `ReviewImage` | Фото к отзыву |
| `ReviewHelpful` | Голос «полезный отзыв» |

### `promotions` — маркетинг

| Сущность | Назначение |
|----------|------------|
| `Promotion` | Маркетинговая кампания |
| `Coupon` | Промокод (% или фикс, лимиты использования) |
| `CouponUsage` | История применения |
| `FlashSale` | Ограниченная по времени распродажа |

---

## 7. Сервисы в Docker Compose

| Сервис | Образ / сборка | Порт | Назначение |
|--------|----------------|------|------------|
| `db` | `postgres:16-alpine` | 5432 | Основная БД, healthcheck `pg_isready` |
| `redis` | `redis:7-alpine` | 6379 | Кэш, брокер Celery |
| `backend` | `./backend` Dockerfile | 8000 | Django + Gunicorn (4 workers) |
| `celery_worker` | `./backend` Dockerfile | — | Фоновые задачи (concurrency 4) |
| `celery_beat` | `./backend` Dockerfile | — | Периодические задачи (DB scheduler) |
| `frontend` | `./frontend` Dockerfile | — | Сборка SPA → volume `frontend_dist` |
| `nginx` | `nginx:1.25-alpine` | 80, 443 | Единая точка входа |

**Именованные тома:** `postgres_data`, `redis_data`, `static_volume`, `media_volume`, `frontend_dist`.

---

## 8. Быстрый старт (локально, Docker)

### Требования

- [Docker](https://docs.docker.com/get-docker/) и Docker Compose v2+
- [Git](https://git-scm.com/)
- 4+ GB свободной RAM

### Шаг 1 — клонирование и конфигурация

```bash
git clone https://github.com/NodirOdilov/FashionFlow.git
cd FashionFlow
cp .env.example .env
```

Отредактируйте `.env`: как минимум задайте надёжный `SECRET_KEY` и пароль PostgreSQL.

### Шаг 2 — запуск стека

```bash
docker compose up --build -d
```

При старте `backend` автоматически выполняет `migrate` и `collectstatic`.

### Шаг 3 — создание суперпользователя

```bash
docker compose exec backend python manage.py createsuperuser
```

### Шаг 4 — доступ к приложению

| Сервис | URL |
|--------|-----|
| **Витрина (SPA)** | http://localhost |
| **REST API** | http://localhost/api/ |
| **Django Admin** | http://localhost/admin/ |
| **Swagger UI** | http://localhost/api/docs/ |
| **OpenAPI Schema** | http://localhost/api/schema/ |
| **Backend напрямую** | http://localhost:8000 (dev) |

---

## 9. Основные команды

### Docker Compose

```bash
# Запуск в фоне
docker compose up -d

# Пересборка после изменений
docker compose up --build -d

# Логи всех сервисов
docker compose logs -f

# Логи только backend
docker compose logs -f backend

# Остановка
docker compose down

# Остановка с удалением томов БД (осторожно!)
docker compose down -v
```

### Django (внутри контейнера)

```bash
# Миграции
docker compose exec backend python manage.py migrate

# Shell
docker compose exec backend python manage.py shell

# Создание суперпользователя
docker compose exec backend python manage.py createsuperuser

# Тесты
docker compose exec backend python manage.py test

# Статика
docker compose exec backend python manage.py collectstatic --noinput
```

### Качество кода

```bash
docker compose exec backend flake8 .
docker compose exec backend black --check .
docker compose exec backend pytest
```

---

## 10. Ручной запуск frontend и backend

Используйте, когда нужен hot-reload без пересборки Docker-образов.

### Backend (локально)

```bash
cd backend
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

export DJANGO_SETTINGS_MODULE=config.settings.development
export DATABASE_URL=postgres://fashionflow:fashionflow_secret@localhost:5432/fashionflow
export REDIS_URL=redis://localhost:6379/0

python manage.py migrate
python manage.py runserver 0.0.0.0:8000
```

> PostgreSQL и Redis должны быть доступны (через `docker compose up db redis -d` или локальные инстансы).

### Celery (опционально)

```bash
# Worker
celery -A config worker -l info

# Beat
celery -A config beat -l info --scheduler django_celery_beat.schedulers:DatabaseScheduler
```

### Frontend (hot reload)

```bash
cd frontend
npm install
npm run dev
```

Vite dev-сервер по умолчанию: **http://localhost:5173**. Убедитесь, что `CORS_ALLOWED_ORIGINS` в `.env` включает этот origin.

```bash
# Production-сборка
npm run build
npm run preview
```

---

## 11. Конфигурация и переменные окружения

Полный шаблон — в файле [`.env.example`](.env.example). Ключевые группы:

### Django

| Переменная | Описание | Пример |
|------------|----------|--------|
| `DJANGO_SETTINGS_MODULE` | Модуль настроек | `config.settings.development` |
| `SECRET_KEY` | Криптографический ключ | *обязательно сменить в prod* |
| `DEBUG` | Режим отладки | `False` в продакшене |
| `ALLOWED_HOSTS` | Разрешённые хосты | `localhost,yourdomain.com` |
| `CORS_ALLOWED_ORIGINS` | Origins для SPA | `http://localhost:5173` |

### База данных и кэш

| Переменная | Описание |
|------------|----------|
| `POSTGRES_DB` / `POSTGRES_USER` / `POSTGRES_PASSWORD` | Учётные данные PostgreSQL |
| `DATABASE_URL` | DSN для django-environ |
| `REDIS_URL` | Redis для кэша, сессий и Celery |

### Платежи (Stripe)

| Переменная | Описание |
|------------|----------|
| `STRIPE_PUBLIC_KEY` | Публичный ключ (frontend) |
| `STRIPE_SECRET_KEY` | Секретный ключ (backend) |
| `STRIPE_WEBHOOK_SECRET` | Подпись webhook-событий |

### Медиа (AWS S3)

| Переменная | Описание |
|------------|----------|
| `AWS_ACCESS_KEY_ID` | IAM access key |
| `AWS_SECRET_ACCESS_KEY` | IAM secret |
| `AWS_STORAGE_BUCKET_NAME` | Имя bucket для медиа |
| `AWS_S3_REGION_NAME` | Регион S3 |

### Прочее

| Переменная | Описание |
|------------|----------|
| `EMAIL_*` | SMTP для транзакционных писем |
| `SENTRY_DSN` | Мониторинг ошибок |
| `SITE_URL` / `FRONTEND_URL` | Базовые URL для ссылок в письмах |

---

## 12. API, очереди и интеграции

### Аутентификация

| Метод | Endpoint | Описание |
|-------|----------|----------|
| `POST` | `/api/accounts/register/` | Регистрация |
| `POST` | `/api/accounts/login/` | Получение JWT-пары |
| `POST` | `/api/accounts/token/refresh/` | Обновление access-токена |
| `GET` | `/api/accounts/profile/` | Профиль текущего пользователя |
| `PUT` | `/api/accounts/style-profile/` | Обновление стилевого профиля |

### Каталог

| Метод | Endpoint | Описание |
|-------|----------|----------|
| `GET` | `/api/products/` | Список товаров (фильтры, поиск, сортировка) |
| `GET` | `/api/products/{slug}/` | Детальная карточка |
| `GET` | `/api/products/categories/` | Дерево категорий |
| `GET` | `/api/products/collections/` | Коллекции |
| `GET` | `/api/products/collections/{slug}/` | Детали коллекции |

### Заказы

| Метод | Endpoint | Описание |
|-------|----------|----------|
| `POST` | `/api/orders/` | Создание заказа |
| `GET` | `/api/orders/` | Список заказов пользователя |
| `GET` | `/api/orders/{id}/` | Детали заказа |
| `POST` | `/api/orders/{id}/cancel/` | Отмена |
| `POST` | `/api/orders/{id}/return/` | Заявка на возврат |

### Образы и отзывы

| Метод | Endpoint | Описание |
|-------|----------|----------|
| `GET` / `POST` | `/api/outfits/` | CRUD образов |
| `GET` / `POST` | `/api/outfits/boards/` | Доски стиля |
| `GET` | `/api/reviews/?product={id}` | Отзывы товара |
| `POST` | `/api/reviews/` | Создание отзыва |

### Промо

| Метод | Endpoint | Описание |
|-------|----------|----------|
| `POST` | `/api/promotions/validate/` | Валидация купона |
| `GET` | `/api/promotions/flash-sales/` | Активные flash-sale |

### Celery-задачи

| Задача | Триггер | Действие |
|--------|---------|----------|
| `send_order_confirmation_email` | После оплаты | Письмо с деталями заказа |
| `send_shipping_notification` | Статус «shipped» | Трек-номер и ETA |
| `send_abandoned_cart_reminders` | Beat (периодически) | Напоминание о неоплаченном заказе |

### Внешние интеграции

| Сервис | Назначение |
|--------|------------|
| **Stripe** | Оплата, webhooks, refunds |
| **AWS S3** | Хранение изображений товаров и отзывов |
| **SMTP** | Транзакционные email |
| **Sentry** | Трекинг ошибок в production |

---

## 13. Мониторинг и эксплуатация

| Область | Рекомендация |
|---------|--------------|
| **Логи** | `docker compose logs -f backend celery_worker` |
| **Ошибки** | Sentry через `SENTRY_DSN` |
| **БД** | Регулярные бэкапы `postgres_data`, мониторинг размера |
| **Redis** | Алерт на memory usage, persistence при необходимости |
| **Медиа** | S3 lifecycle policies, CDN перед bucket |
| **SSL** | Certbot / Let's Encrypt на Nginx (порт 443 уже проброшен) |

### Healthchecks

PostgreSQL и Redis в `docker-compose.yml` имеют встроенные healthcheck — `backend`, `celery_worker` и `celery_beat` стартуют только после готовности зависимостей.

---

## 14. CI/CD

В репозитории пока **нет** готового GitHub Actions workflow. Рекомендуемый pipeline:

```yaml
# .github/workflows/ci.yml (рекомендуемый шаблон)
# 1. lint: flake8 + black --check
# 2. test: pytest + manage.py test
# 3. build: docker compose build
# 4. deploy: push образов → docker compose pull на сервере
```

### Чеклист деплоя в production

1. Скопировать `.env.example` → `.env`, заполнить все секреты.
2. Установить `DJANGO_SETTINGS_MODULE=config.settings.production`.
3. Установить `DEBUG=False`, надёжный `SECRET_KEY`.
4. `docker compose up --build -d`
5. `docker compose exec backend python manage.py migrate`
6. `docker compose exec backend python manage.py createsuperuser`
7. Настроить DNS → сервер, выпустить SSL-сертификат.
8. Настроить Stripe webhooks на `https://yourdomain.com/api/...`.

---

## 15. Безопасность и хранение медиа

| Мера | Реализация |
|------|------------|
| **Аутентификация** | JWT (access 30 мин, refresh 7 дней, rotation + blacklist) |
| **Авторизация** | DRF permissions, owner-only для заказов и профиля |
| **Rate limiting** | 100 req/h (anon), 1000 req/h (user) |
| **CORS** | Явный whitelist origins |
| **Пароли** | Django validators, минимум 8 символов |
| **CSRF** | Включён для session-auth; API — JWT Bearer |
| **XSS в UGC** | `bleach` для пользовательского контента |
| **Медиа** | Локальный том (dev) или S3 + подписанные URL (prod) |
| **Секреты** | Только через `.env`, файл в `.gitignore` |

> **Важно:** никогда не коммитьте `.env` с реальными ключами Stripe, AWS и `SECRET_KEY`.

---

## 16. Роли компонентов в продакшене

```
                    ┌─────────────────────────────────────┐
                    │           Internet / CDN            │
                    └──────────────────┬──────────────────┘
                                       │
                    ┌──────────────────▼──────────────────┐
                    │  Nginx (TLS termination, gzip,      │
                    │  rate limit, static/media cache)    │
                    └───────┬──────────────────┬──────────┘
                            │                  │
              ┌─────────────▼──────┐   ┌───────▼──────────┐
              │  Vue SPA (static)  │   │  Gunicorn/Django │
              │  frontend_dist     │   │  REST + Admin    │
              └────────────────────┘   └────────┬─────────┘
                                                │
                         ┌──────────────────────┼──────────────────────┐
                         │                      │                      │
              ┌──────────▼─────────┐ ┌──────────▼────────┐ ┌──────────▼────────┐
              │  Celery Workers    │ │  PostgreSQL (HA)  │ │  Redis (Sentinel) │
              │  email, reminders  │ │  primary + replica│ │  cache + broker   │
              └────────────────────┘ └───────────────────┘ └───────────────────┘
                         │
              ┌──────────▼─────────┐
              │  AWS S3 + CloudFront│
              │  product images     │
              └────────────────────┘
```

| Компонент | Масштабирование | Примечание |
|-----------|-----------------|------------|
| **Nginx** | Несколько инстансов за LB | Terminate SSL, кэш static 30d |
| **Gunicorn** | `--workers` ≈ 2×CPU+1 | Stateless, горизонтально |
| **Celery** | Увеличить `--concurrency` / реплики | IO-bound задачи (email) |
| **PostgreSQL** | Read-replica для аналитики | Connection pooling (PgBouncer) |
| **Redis** | Cluster / Sentinel | Отдельные DB для cache и broker |

---

## 17. Лицензия

Проект распространяется как **проприетарное программное обеспечение**. Все права защищены.

Использование, копирование и распространение без письменного разрешения правообладателя запрещены.

---

## 18. Поддержка

| Канал | Действие |
|-------|----------|
| **Issues** | [GitHub Issues](https://github.com/NodirOdilov/FashionFlow/issues) — баги и предложения |
| **API Docs** | http://localhost/api/docs/ после запуска |
| **Admin** | http://localhost/admin/ — операционное управление |

---

<div align="center">

**FashionFlow** — стиль, технологии и commerce в одной платформе.

*Сделано с вниманием к деталям.*

</div>
