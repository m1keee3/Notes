# 

## Что такое Dockerfile?

**Dockerfile** — это скрипт, который содержит инструкции для создания образа Docker. Он определяет базовый образ, устанавливает переменные среды, ПО и настраивает контейнер для приложения.

---

### Синтаксис Dockerfile

| Инструкция | Описание | Пример |
|------------|----------|--------|
| `FROM` | Базовый образ | `FROM ubuntu:20.04` |
| `WORKDIR` | Рабочая директория | `WORKDIR /app` |
| `COPY` | Копирование файлов в контейнер | `COPY . /app` |
| `RUN` | Выполнение команд в образе | `RUN apt-get update` |
| `ENV` | Установка переменных окружения | `ENV NODE_ENV=production` |
| `EXPOSE` | Объявление порта | `EXPOSE 8080` |
| `CMD` | Команда по умолчанию для запуска | `CMD ["npm", "start"]` |
| `ARG` | Переменная для сборки | `ARG VERSION=latest` |
| `ENTRYPOINT` | Исполняемый файл контейнера | `ENTRYPOINT ["node", "app.js"]` |
| `VOLUME` | Точка монтирования тома | `VOLUME /data` |
| `LABEL` | Метаданные образа | `LABEL version="1.0"` |
| `USER` | Пользователь для запуска | `USER app` |
| `ADD` | Копирование + распаковка архивов | `ADD app.tar.gz /app` |

---

### Пример Dockerfile

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
ENV NODE_ENV=production
CMD ["node", "app.js"]
```

Запуск **Go приложения**

```dockerfile
FROM golang:1.21-alpine  
  
WORKDIR /app  
  
RUN apk add --no-cache git  
  
COPY go.mod go.sum ./  
RUN go mod download  
  
COPY . .  
  
RUN go build -o simulator main.go  
  
EXPOSE 2112  
  
CMD ["./simulator"]
```
