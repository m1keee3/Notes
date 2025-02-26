Интерфейс в Go — это набор методов (сигнатур функций), которые должны быть реализованы типом, чтобы он считался удовлетворяющим этому интерфейсу.

#### Встраивание интерфейсов

```
type Reader interface {
	Read(p []byte) (n int, err error)
}

type Closer interface {
	Close() error
}

type ReadCloser interface {
	Reader
	Closer
}
```

#### Пустой интерфейс interface{}

Пустой интерфейс — это интерфейс, который не требует никаких методов. Он может хранить значение любого типа, поскольку все типы реализуют как минимум нулевое количество методов.
Пустой интерфейс часто используется в функциях, которые должны работать с данными любого типа, например, `fmt.Println`.

```
func Println(a ...interface{}) { ... }
```

**Утверждение типа (type assertion)** - это извлечение определенного типа из пустого интерфейса

```
var i interface{} = "hello"

s := i.(string)
fmt.Println(s)

// Если тип не совпадает, будет паника
```

Чтобы избежать паники надо обрабатывать возможную ошибку:

```
s, ok := i.(string)
if ok {
    fmt.Println(s)
} else {
    fmt.Println("Не string!")
}
```

#### Переключение типов (Type Switch)

```
func Describe(v interface{}) {
    switch v := v.(type) {
    case int:
        fmt.Println("Это int:", v)
    case string:
        fmt.Println("Это string:", v)
    default:
        fmt.Println("Неизвестный тип")
    }
}
```
