---
tags: [finly, project, go, kafka, postgresql, redis, grpc]
---
# Finly — Technology Stack Reference

Карта технологий проекта Finly. Смотри [[README]] для архитектурного обзора.

## Языки
- [[Study/backend/Go/MOC|Go]] — Scanner, Watcher, Users, API Gateway написаны на Go
  - [[Goroutine|Goroutines]] — конкурентный поиск паттернов в Scanner
  - [[Каналы (Channels) Внутреннее устройство|Channels]] — коммуникация между горутинами
  - [[Context|Context]] — отмена запросов и таймауты в gRPC вызовах

## Коммуникация
- [[API|gRPC & REST]] — gRPC между сервисами; Gateway выставляет REST
  - [[HTTP|HTTP/2]] — gRPC работает поверх HTTP/2
  - [[TCP & UDP|TCP]] — транспорт для gRPC

## Брокер сообщений
- [[Kafka|Kafka]] — Watcher публикует уведомления в Kafka topics
  - Гарантия доставки: At-least-once (рекомендуется идемпотентный consumer)
  - Ключ партиции: user_id для упорядоченных уведомлений

## Базы данных
- [[Реляционные базы данных|PostgreSQL]] — Watcher хранит лучшие графики
  - [[Транзакции, ACID, Уровни изоляции|Транзакции]] — целостность данных графиков
  - [[MVCC, Vacuum & Autovacuum|MVCC]] — конкурентное чтение/запись в Watcher
  - [[Индексы|Индексы]] — B-tree по ticker symbol и timestamp
- [[Redis|Redis]] — Scanner кэширует результаты; Gateway кэширует частые ответы

## Инфраструктура
- [[Study/backend/Docker/MOC|Docker]] — все сервисы контейнеризованы
  - [[Docker Compose|Docker Compose]] — локальная разработка
  - [[Kubernetes|Kubernetes]] — целевой продакшн

## Архитектура
- [[Study/backend/Архитектуры/MOC|Architecture Patterns]] — микросервисы соответствуют Clean/Hexagonal архитектуре

## Сервисы
- [[Scanner]] — Go сервис: поиск паттернов + Redis кэш
- [[Watcher]] — Go сервис: мониторинг + PostgreSQL + Kafka producer
