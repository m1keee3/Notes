---
tags: [networks, moc, backend]
---

# Networks — Map of Content

## Основы
- [[OSI]] — 7-уровневая модель, инкапсуляция, MAC vs IP
- [[TCP & UDP]] — транспортный уровень: 3-way handshake, надёжность vs скорость
- [[TCP IP]] — стек TCP/IP
- [[HTTP]] — HTTP/1.1, методы, коды статусов, заголовки, HTTPS
- [[API]] — REST vs gRPC, protobuf, HTTP/2

## Инфраструктура
- [[Порты]] — известные порты, Docker port forwarding
- [[Firewall]] — пакетная фильтрация
- [[Network Protocol Dependency]] — зависимости протоколов
- [[Как работает браузер]] — жизненный цикл запроса: DNS → TCP → HTTP
- [[Типы подключения ВМ к сети]] — NAT, Bridged, Internal, Host-only

## Related
- [[Study/backend/Архитектуры/MOC|Architecture MOC]] — выбор API стиля — архитектурное решение
- [[Study/backend/Docker/MOC|Docker & K8s MOC]] — сетевое взаимодействие контейнеров строится на OSI/TCP
- [[Projects/Finly/README|Finly]] — gRPC между сервисами, REST на Gateway
