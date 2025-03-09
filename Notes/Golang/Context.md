- **Отмена операций** – позволяет прервать выполнение горутин при истечении времени или внешнем сигнале.
	
- **Передача данных** – можно передавать значения между функциями.
	
- **Дедлайны и тайм-ауты** – можно задать временные ограничения на выполнение задач.

### Создание контекста

В `context` есть несколько способов его создания:

##### 1. `context.Background()`

Используется как корневой контекст.

```
ctx := context.Background()
```

##### 2. `context.TODO()`

Применяется, если пока не ясно, какой контекст использовать.

```
ctx := context.TODO()
```

##### 3. `context.WithCancel(parent)`

Создаёт контекст, который можно отменить вручную.

```
ctx, cancel := context.WithCancel(context.Background())

fmt.Println(ctx.Err()) // <nil> еще нет ошибки контекста

go func() {
    time.Sleep(2 * time.Second)
    cancel() // Отмена контекста
}()

<-ctx.Done()
fmt.Println(ctx.Err()) // context canceled 
```

##### 4. `context.WithTimeout(parent, duration)`

Контекст отменяется через указанное время.

```
ctx, cancel := context.WithTimeout(context.Background(), 3*time.Second)
defer cancel() // Важно вызывать cancel(), чтобы избежать утечек

select {
case <-time.After(5 * time.Second):
    fmt.Println("Операция завершена")
case <-ctx.Done():
    fmt.Println("Контекст завершился:", ctx.Err())
}
```

##### 5. `context.WithDeadline(parent, time)`

Похож на `WithTimeout`, но задаёт точный момент завершения.

```
deadline := time.Now().Add(2 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
defer cancel()

select {
case <-time.After(3 * time.Second):
    fmt.Println("Операция выполнена")
case <-ctx.Done():
    fmt.Println("Контекст завершён:", ctx.Err())
}

```

##### 6. `context.WithValue(parent, key, value)`

Позволяет передавать и хранить значения в контексте.

```
ctx := context.WithValue(context.Background(), "userID", 42)

func process(ctx context.Context) {
    if v := ctx.Value("userID"); v != nil {
        fmt.Println("UserID:", v)
    }
}

process(ctx)

```


