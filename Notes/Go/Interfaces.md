Интерфейс в Go — это набор методов (сигнатур функций), которые должны быть реализованы типом, чтобы он считался удовлетворяющим этому интерфейсу.

Рассмотрим  такой пример:

```
type Word struct {  
    name     string  
    priority uint  
}  
  
type Foo interface {  
    foo()  
}  
  
func (w *Word) foo() {  
    fmt.Println("call foo()")  
}  
  
func call(f Foo) {  
    if f != nil {  
       f.foo()  
    } else {  
       fmt.Println("f null")  
    }  
}  
  
func main() {  
    var f1 *Word  
    call(f1)  
}

// Вывод 
// call foo()
```

Для того чтобы понять почему так происходит разберемся с интерфейсом.

Интерфейсная переменная содержит два компонента:

- **Динамический тип** (тип реализации).
    
- **Динамическое значение** (конкретное значение).

```
type iface struct {
	tab  *itab
	data unsafe.Pointer
}
```

`data` - это непосредственно наш f1 из примера. Вся информация по методам, типам и прочему скрывается в `tab` - интерфейсной таблицей (interface table).

```
type itab struct {
	inter *interfacetype
	_type *_type
	hash  uint32 // copy of _type.hash. Used for type switches.
	_     [4]byte
	fun   [1]uintptr // variable sized. fun[0]==0 means _type does not implement inter.
}
```

`_type` - это метаданные о динамическом типе.

Таким образом интерфейс будет `nil` только в том случае если и `data` и `_type` будут `nil`.
В примере же переменная знает свой тип (`*Word`) то есть `_type`не равен `nil`, а `data` не инициализирован (`=nil')

**Почему не программа не паникует?** 

Метод вызывается таким образом:

```
f.tab.fun[0](f.data)
```

`fun[0]` - единственный метод интерфейса

Так как этот метод не использует поля класса то `f.data` которая `nil` не используется и программа корректно завершит работу.

Если изменим реализацию метода - то все упадет как надо:

```
func (w *Word) foo() {
	fmt.Printf("call foo(): %s\n", w.name)
}
```


---

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


---

#### Утверждение типа (type assertion)

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


---

#### Errors

**Error interface** позволяет реализовывать кастомные error

```
type error interface {
    Error() string
}
```