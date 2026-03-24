# docker-nginx-python-proxy

Простой HTTP-бэкенд на Python за nginx, поднимается через Docker Compose.

## Запуск

В корне репозитория:

```bash
docker compose up -d --build
```

Если порт 80 занят, скопируйте `.env.example` в `.env` и задайте свободный `HTTP_PORT` (например `8080`).

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
