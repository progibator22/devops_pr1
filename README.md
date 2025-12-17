# Redis Docker Container Project

**Redis Docker Container Project** — демонстрационный Docker-образ для быстрого и воспроизводимого развёртывания Redis в контейнеризованной среде.  
Проект ориентирован на **локальную разработку**, **тестирование** и использование в **CI/CD-пайплайнах**.

> **Важно:** образ предназначен для учебных и демонстрационных целей. Для production требуется дополнительная настройка безопасности, мониторинга и отказоустойчивости.

---

## Назначение контейнера

Контейнер решает следующие задачи:

- Быстрый запуск Redis без ручной установки
- Конфигурация через переменные окружения
- Персистентное хранение данных через Docker volumes
- Простая интеграция с Docker Compose

---

## Состав проекта

- `Dockerfile` — сборка образа на базе `redis:7-alpine`
- `entrypoint.sh` — генерация `redis.conf` на основе ENV
- `docker-compose.yml` — пример запуска
- `README.md` — документация

![Redis Logo](https://upload.wikimedia.org/wikipedia/commons/6/6b/Redis_Logo.svg)

---

## Поддерживаемые аргументы образа

| Переменная окружения | Обязательная | Описание | Пример |
|---|---:|---|---|
| `REDIS_PASSWORD` | Нет | Включает аутентификацию Redis (`requirepass`) | `secret123` |
| `REDIS_PORT` | Нет | Порт Redis внутри контейнера | `6379` |
| `REDIS_BIND` | Нет | IP для биндинга | `0.0.0.0` |
| `REDIS_APPENDONLY` | Нет | Включение AOF (`yes` / `no`) | `yes` |
| `REDIS_MAXMEMORY` | Нет | Лимит памяти Redis | `256mb` |
| `REDIS_MAXMEMORY_POLICY` | Нет | Политика вытеснения ключей | `allkeys-lru` |
| `TZ` | Нет | Часовой пояс контейнера | `Europe/Moscow` |

---

## Настройка через переменные окружения

### Основные параметры

- **`REDIS_PASSWORD`**
  - При указании включает защиту паролем
  - Клиенты обязаны выполнять `AUTH <password>`

- **`REDIS_PORT`**
  - Порт Redis внутри контейнера
  - Значение по умолчанию: `6379`

- **`REDIS_BIND`**
  - Адрес, на котором Redis принимает соединения

### Персистентность и память

- **`REDIS_APPENDONLY`**
  - `yes` — включить AOF
  - `no` — отключить AOF

- **`REDIS_MAXMEMORY`**
  - Ограничение используемой памяти

- **`REDIS_MAXMEMORY_POLICY`**
  - `noeviction`
  - `allkeys-lru`
  - `volatile-lru`

---

## Пример запуска контейнера

```bash
docker run -d \
  --name redis-demo \
  -p 6379:6379 \
  -e REDIS_PASSWORD=secret123 \
  -e REDIS_APPENDONLY=yes \
  -e REDIS_MAXMEMORY=256mb \
  -v redis_data:/data \
  redis-demo:latest

version: "3.8"

services:
  redis:
    image: redis-demo:latest
    container_name: redis-demo
    ports:
      - "6379:6379"
    environment:
      REDIS_PASSWORD: "secret123"
      REDIS_APPENDONLY: "yes"
      REDIS_MAXMEMORY: "256mb"
    volumes:
      - redis_data:/data

volumes:
  redis_data:
```
