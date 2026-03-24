# docker-nginx-python-proxy

Простой HTTP-бэкенд на Python за nginx, поднимается через Docker Compose.

**Нужно:** Docker и плагин Docker Compose v2 (`docker compose`).

## Запуск

1. Клонировать репозиторий и перейти в каталог проекта.
2. (По желанию) Если порт 80 занят: скопировать `.env.example` в `.env` и задать свободный `HTTP_PORT` (например `8080`).
3. Выполнить:

```bash
docker compose up -d --build
```

4. Дождаться статуса `healthy` у контейнеров: `docker compose ps`.

## Проверка

```bash
curl http://localhost
```

При другом порте:

```bash
curl "http://localhost:${HTTP_PORT}"
```

Ожидаемый ответ:

```text
Hello from Effective Mobile!
```

## Как устроено

Наружу проброшен только nginx. Запрос с хоста попадает в контейнер nginx; он проксирует `/` на сервис `backend` по адресу `backend:8080` во внутренней сети Docker. Порт 8080 бэкенда на хост не публикуется.

```text
curl  -->  хост:${HTTP_PORT:-80}  -->  nginx (em-nginx)
                                           |
                                           v
                                     backend:8080 (em-backend)
```

Стек: Docker, Docker Compose, nginx, Python 3.
