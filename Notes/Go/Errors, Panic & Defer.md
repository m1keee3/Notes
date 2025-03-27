### Errors

```go
type error interface {
    Error() string // возвращает строковое описание ошибки
}
```

**Пример функции использующей Error**

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero") // в пакете errors
    }
    return a / b, nil
}
```

**Пример реализации своего класса error**

```go
type DivisionError struct {
    dividend int
    divisor  int
}

func (e *DivisionError) Error() string {
    return fmt.Sprintf("cannot divide %d by %d", e.dividend, e.divisor)
}
```

**Существует возможность обертывания ошибки с добавлением в нее контекста при помощи fmt.Errorf**

```go
func step1() error {
    return errors.New("error in step1")
}

func step2() error {
    err := step1()
    if err != nil {
        return fmt.Errorf("step2: %w", err)
    }
    return nil
}

func main() {
    err := step2()
    if err != nil {
        fmt.Println("Error:", err)
        // Распаковка обернутой ошибки
        if unwrappedErr := errors.Unwrap(err); unwrappedErr != nil {
            fmt.Println("Unwrapped Error:", unwrappedErr)
        }
    }
}
```

#### errors.Is & errors.As

**Is** - Позволяет проверить является ли ошибка или любая из обернутых конкретной ошибкой

**As** - Позволяет проверить на совпадение типов, например при использовании кастомного типа error


---

### Panic

Вызывает аварийное завершение программы. Когда вызывается `panic`, выполнение текущей функции немедленно прекращается, и начинается процесс "раскрутки стека" (unwinding the stack). Это означает, что все отложенные функции (`defer`) выполняются, а затем программа завершается с сообщением об ошибке.

Стоит использовать только в исключительных ситуациях, когда программа не может восстановиться из-за ошибки


### Defer

Позволяет отложить выполнение функции до момента завершения окружающей функции. Это полезно для выполнения очистки ресурсов, таких как закрытие файлов, освобождение мьютексов или соединений с базой данных.
##### Особенности `defer`:

1. **Отложенное выполнение**: Функция, вызванная с `defer`, выполняется только после завершения окружающей функции.
    
2. **LIFO (Last In, First Out)**: Если в функции несколько `defer`, они выполняются в обратном порядке (последний добавленный `defer` выполняется первым).
    
3. **Аргументы вычисляются сразу**: Аргументы функции, переданной в `defer`, вычисляются в момент вызова `defer`, а не в момент выполнения.

**Пример:**

```go
a := 1  
defer fmt.Println("first defer", a)  
defer fmt.Println("second defer", a)  
  
a = 2  
fmt.Println(a)

// Вывод

2
second defer 1
first defer 1
```


#### Интересный пример с замыканием

```go
func sum(a, b int) (sum int) {
	defer func() {
		sum *= 2
	}

	sum = a + b
	return
}

fmt.Print(sum(2, 3)) // 10
```

---

### Panic & Defer

Когда происходит `panic`, все отложенные функции (`defer`) в текущей функции выполняются перед завершением программы. Это делает `defer` полезным для очистки ресурсов даже в случае аварийного завершения.

**Пример:**

```go
func cleanup() {
    fmt.Println("Cleaning up resources...")
}

func riskyOperation() {
    defer cleanup() // Очистка ресурсов будет выполнена даже при panic
    fmt.Println("Performing risky operation")
    panic("Something went wrong!") // Вызов panic
}

func main() {
    fmt.Println("Start")
    riskyOperation()
    fmt.Println("This will not be printed") // Этот код не выполнится
}

//Вывод

Start
Performing risky operation
Cleaning up resources...
panic: Something went wrong!
```


### Recover

Это встроенная функция, которая позволяет восстановить управление после `panic`. Она работает только внутри отложенных функций (`defer`). Если `recover` вызывается во время раскрутки стека после `panic`, она останавливает процесс раскрутки и возвращает значение, переданное в `panic`.

**Пример:**

```go
func handlePanic() {
    if r := recover(); r != nil {
        fmt.Println("Recovered from panic:", r)
    }
}

func riskyOperation() {
    defer handlePanic() // Отложенный вызов для восстановления
    fmt.Println("Performing risky operation")
    panic("Something went wrong!") // Вызов panic
}

func main() {
    fmt.Println("Start")
    riskyOperation()
    fmt.Println("End") // Этот код выполнится, так как panic была восстановлена
}

// Вывод

Start
Performing risky operation
Recovered from panic: Something went wrong!
End
```

Функция `recover()` возвращает значение переданное в `panic()`