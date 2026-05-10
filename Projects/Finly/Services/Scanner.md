---
tags: [finly, go, grpc, redis]
---

# Scanner Service

Сервис поиска паттернов в данных ценных бумаг.

Смотри [[Projects/Finly/README]] для общей архитектуры и [[TECH]] для деталей стека.

## Технологии
- [[Study/backend/Go/MOC|Go]] — язык реализации
- [[Redis|Redis]] — кэш результатов поиска паттернов
- [[API|gRPC]] — API для взаимодействия с Gateway
