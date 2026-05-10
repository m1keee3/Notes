---
tags: [databases, moc, backend]
---

# Databases — Map of Content

## Реляционные / SQL
- [[Реляционные базы данных]] — реляционная модель, OLTP
- [[SQL]] — SELECT, JOIN, синтаксис
- [[Транзакции, ACID, Уровни изоляции|Transactions & ACID]] — уровни изоляции, аномалии
- [[Индексы]] — B-tree, Hash, GiST, GIN
- [[Ключи]] — первичные, внешние, уникальные ключи
- [[Нормализация]] — нормальные формы
- [[Constraint]] — ограничения БД
- [[Планировщик]] — query planner

## Архитектура PostgreSQL
- [[Фоновые процессы]] — Postmaster, WAL Writer, Checkpointer, Autovacuum
- [[MVCC, Vacuum & Autovacuum|MVCC & Vacuum]] — версионирование, очистка мёртвых tuple
- [[Обработка SQL запроса|Query Processing]] — Parse → Plan → Execute
- [[Модель доступа|Access Model]] — роли, привилегии, схемы
- [[Распределенные транзакции]] — 2PC, распределённые транзакции

## NoSQL
- [[NoSQL базы данных]] — типы: columnar, document, graph, key-value, timeseries, BASE vs ACID
- [[Redis]] — key-value хранилище, кэширование
- [[MongoDB]] — document store

## Распределённые системы
- [[Распределенные базы данных]] — CAP теорема, репликация, шардинг
- [[Метрики]] — Prometheus & Grafana

## Брокеры сообщений
- [[Kafka]] — topics, partitions, delivery guarantees, репликация

## Related
- [[Projects/Finly/README|Finly]] — использует PostgreSQL, Redis, Kafka
- [[Study/backend/Go/MOC|Go MOC]] — Go драйверы для работы с БД
