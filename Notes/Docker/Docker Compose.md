## Что такое Docker Compose?

Docker Compose — это инструмент для определения и запуска многоконтейнерных приложений Docker. С помощью YAML-файла можно описать все сервисы, сети, тома и зависимости между контейнерами, а затем управлять всем стеком одной командой.

---

## Синтаксис Docker Compose

### version:
Указывает версию формата файла Docker Compose.

**version: '3.9'**

---

### services:
Определяет сервисы/контейнеры, которые составляют приложение.

```yaml
services:
  web:
    image: nginx:latest
  api:
    build: ./api
```


---

### build:

Настройка контекста сборки и Dockerfile для сервиса.

```yaml
build:
  context: .
  dockerfile: Dockerfile.dev
```


---

### image:

Указывает образ для запуска контейнера.

**image: postgres:13**

---

### ports:

Маппинг портов хоста на порты контейнера.

**ports: - "8080:80"**

---

### environment:

Устанавливает переменные окружения для сервиса.

```yaml
environment:
  - NODE_ENV=production
  - DB_HOST=db
```


---

### volumes:

Определяет точки монтирования томов.

```yaml
volumes:
  - db_data:/var/lib/data
  - /host/path:/container/path
```


---

### networks:

Подключение сервиса к сетям.

```yaml
networks:
  - frontend
  - backend
```


---

### depends_on:

Указывает зависимости между сервисами.

```yaml
depends_on:
  - db
  - redis
```


---

### command:

Переопределяет команду запуска по умолчанию.

**command: ["npm", "start"]**

---

### volumes_from:

Подключает volumes от другого сервиса.

```yaml
volumes_from:
  - service_name
```


---

## Пример файла Docker Compose

```yaml
version: '3.8'

services:
  # MongoDB сервис
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: admin

  # Node.js API сервис
  api:
    build:
      context: ./api
      dockerfile: Dockerfile
    ports:
      - "5000:5000"
    depends_on:
      - mongo
    environment:
      MONGO_URI: mongodb://admin:admin@mongo:27017/mydatabase
    networks:
      - app_network

  # React клиент
  client:
    build:
      context: ./client
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    depends_on:
      - api
    networks:
      - app_network
```
