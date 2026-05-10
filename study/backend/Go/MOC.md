---
tags: [go, moc, backend]
---

# Go — Map of Content

## Основы языка
- [[Память]] — Arena, Pages, Spans, escape analysis, zero values
- [[Interfaces]] — iface internals, itab, type assertions
- [[Generics]] — type parameters, constraints (Go 1.18+)
- [[Замыкания (Closures)|Closures]] — замыкания и захват переменных
- [[Slices]] — внутреннее устройство слайсов
- [[Map]] — внутреннее устройство map
- [[JSON]] — encoding/decoding
- [[go.mod & go.sum]] — модульная система
- [[Errors, Panic & Defer]] — обработка ошибок и defer

## Конкурентность
- [[Goroutine|Goroutine]] — g struct, стек, жизненный цикл
- [[Планировщик (Scheduler)|Go Scheduler]] — GMP модель, work stealing, sysmon
- [[Каналы (Channels) Внутреннее устройство|Channels]] — hchan internals, send/recv очереди
- [[Context]] — отмена, таймауты, дедлайны
- [[Concurency|Concurrency Patterns]] — WaitGroup, Mutex, Pipeline, Worker Pool
- [[Garbage Collector]] — mark-and-sweep, tri-color, write barriers

## Фреймворки
- [[Echo|Echo]] — веб-фреймворк Echo
- [[Gin|Gin]] — веб-фреймворк Gin

## Related
- [[Study/backend/ОСи/MOC|OS MOC]] — планировщик Go работает поверх OS планировщика
- [[Study/backend/Архитектуры/MOC|Architecture MOC]]
- [[Projects/Finly/README|Finly]] — Go используется во всех сервисах проекта
- [[compute & io bound]] — горутины идеальны для I/O-bound задач
