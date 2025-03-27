### Пример данных в формате JSON

```go
{ 
	"firstName": "Иван",
	"lastName": "Иванов", 
	"address": {
		"streetAddress": "Московское ш., 101, кв.101", 
		"city": "Ленинград", 
		"postalCode": 101101 
	},
	"phoneNumbers": [ 
	"812 123-1234", 
	"916 123-4567" ] 
}
```


### Marshal

В Go для работы с JSON файлами существует две основные функции

```go
json.Marshal()
```

Принимает аргумент типа `interface{}` и возвращает байтовый срез с данными в формате JSON

```go
type myStruct struct {
	Name   string
	Age    int
	Status bool
	Values []int
}

s := myStruct{
	Name:   "John Connor",
	Age:    35,
	Status: true,
	Values: []int{15, 11, 37},
}

data, err := json.Marshal(s)
if err != nil {
	fmt.Println(err)
	return
}

fmt.Printf("%s", data) // {"Name":"John Connor","Age":35,"Status":true,"Values":[15,11,37]}
```


### Unmarshal

Принимает в качестве аргумента байтовый срез и указатель на объект, в который требуется декодировать данные.

```go
data := []byte(`{"Name":"John Connor","Age":35,"Status":true,"Values":[15,11,37]}`)

type myStruct struct {
	Name   string
	Age    int
	Status bool
	Values []int
}

var s myStruct

if err := json.Unmarshal(data, &s); err != nil {
	fmt.Println(err)
	return
}

fmt.Printf("%v", s) // {John Connor 35 true [15 11 37]}
```


### Проверка json на правильность

Мы можем проверить является ли срез байтов форматом json:  
 

```go
type user struct {
	Name     string
	Email    string
	Status   bool
	Language []byte
}

m := user{Name: "John Connor", Email: "test email"}

data, _ := json.Marshal(m)

data = bytes.Trim(data, "{") // испортим json удалив '{'

// функция json.Valid возвращает bool, true - если json правильный
if !json.Valid(data) {
	fmt.Println("invalid json!") // вывод: invalid json!
}

fmt.Printf("%s", data) // вывод: "Name":"John Connor","Email":"test email","Status":false,"Language":null}
```