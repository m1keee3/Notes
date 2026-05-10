---
tags: [go, gc, memory, backend]
---


- Использует алгоритм **Mark and Sweep**
- **Трехцветный** - алгоритм расцветки
- Выполняется **конкурентно** с основной программой с использованием **барьеров записи**

---

## Mark and Sweep

---

## Related
- [[Память]] — модель памяти (Arena, Span, Pages), которой управляет GC
- [[Планировщик (Scheduler)|Scheduler]] — GC использует write barriers, взаимодействующие с safe points планировщика
- [[Goroutine|Goroutine]] — GC сканирует стеки горутин (флаг gcscandone в структуре g)
