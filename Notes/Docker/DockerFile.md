**Dockerfile** — это скрипт с инструкциями для создания образа Docker.

### Основные инструкции:

- **`FROM`** — базовый образ  
  `FROM ubuntu:20.04`

- **`WORKDIR`** — рабочая директория  
  `WORKDIR /app`

- **`COPY`** — копирование файлов в контейнер  
  `COPY . /app`

- **`RUN`** — выполнение команд в образе  
  `RUN apt-get update && apt-get install -y curl`

- **`ENV`** — установка переменных окружения  
  `ENV NODE_ENV=production`

- **`EXPOSE`** — объявление порта  
  `EXPOSE 8080`

- **`CMD`** — команда по умолчанию для запуска  
  `CMD ["npm", "start"]`

- **`ARG`** — переменные для сборки  
  `ARG VERSION=latest`

- **`ENTRYPOINT`** — запуск контейнера как исполняемого файла  
  `ENTRYPOINT ["node", "app.js"]`

- **`VOLUME`** — точка монтирования тома  
  `VOLUME /data`

- **`LABEL`** — метаданные  
  `LABEL version="1.0"`

- **`USER`** — пользователь для запуска  
  `USER app`

- **`ADD`** — копирование с распаковкой архивов  
  `ADD app.tar.gz /app`

---

## 2. Docker Compose

**Docker Compose** — инструмент для управления многоконтейнерными приложениями через YAML-файл.

### Ключевые секции:

- **`version`** — версия формата  
  `version: '3.8'`

- **`services`** — описание сервисов  
  ```yaml
  services:
    web:
      image: nginx:latest

- **`networks`** — настройка сетей
    
    yaml
    
    networks:
      my_network:
        driver: bridge
    
- **`volumes`** — управление томами
    
    yaml
    
    volumes:
      my_volume:
    
- **`environment`** — переменные окружения
    
    yaml
    
    environment:
      NODE_ENV: production
    
- **`ports`** — проброс портов  
    `ports: - "8080:80"`
    
- **`depends_on`** — зависимости между сервисами
    
    yaml
    
    depends_on:
      - db
    
- **`build`** — сборка образа из Dockerfile
    
    yaml
    
    build:
      context: .
      dockerfile: Dockerfile.dev