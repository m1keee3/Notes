### Работа с образами

```bash
docker build -t myapp .          # Собрать образ
docker images                    # Список образов
docker pull nginx:latest         # Скачать образ
docker rmi myapp:latest          # Удалить образ
docker tag myapp:latest myapp:v1 # Добавить тег
docker push myapp:v1             # Загрузить в DockerHub
docker save -o myapp.tar myapp:v1 # Сохранить в архив
docker load -i myapp.tar         # Загрузить из архива
docker image prune               # Удалить неиспользуемые образы
```

### Работа с контейнерами

```bash
docker run myapp                          # Запустить контейнер
docker run --name my_container myapp:v1   # Запустить с именем
docker ps                                 # Список запущенных контейнеров
docker ps -a                              # Все контейнеры
docker stop my_container                  # Остановить
docker start my_container                 # Запустить остановленный
docker run -it my_container sh            # Интерактивный режим
docker rm my_container                    # Удалить остановленный
docker rm -f my_container                 # Принудительно удалить
docker inspect my_container               # Детали контейнера
docker logs my_container                  # Логи
docker pause/unpause my_container         # Приостановить/возобновить
```

### Тома (Volumes)

```bash
docker volume create my_volume            # Создать том
docker volume ls                          # Список томов
docker volume inspect my_volume           # Информация о томе
docker volume rm my_volume                # Удалить том
docker run -v my_volume:/app/data myapp   # Запустить с томом
docker cp data.txt my_container:/app/data # Копировать файлы
```

### Сети

```bash
docker network ls                         # Список сетей
docker network inspect bridge             # Информация о сети
docker network create my_network          # Создать сеть
docker network connect my_network my_container # Подключить контейнер
docker network disconnect my_network my_container # Отключить
```

### Команды Docker Compose

```bash
docker compose up                    # Запуск сервисов
docker compose up -d                 # Запуск в фоне
docker compose down                  # Остановка и удаление
docker compose ps                    # Список контейнеров
docker compose logs                  # Логи всех сервисов
docker compose build                 Пересобрать образы
docker compose run web npm install   # Выполнить команду в сервисе
docker compose pause/unpause web    # Приостановить/возобновить сервис
docker compose up --scale web=3     # Масштабирование сервиса
```
