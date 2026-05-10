---
tags: [go, gin, web, backend]
---

[**Gin**](https://github.com/gin-gonic/gin) — это лёгкий, быстрый и удобный веб-фреймворк для Go, построенный поверх стандартной библиотеки `net/http`.  
Его главная цель — упростить создание **REST API** и веб-приложений.

---

## 🔹 Особенности Gin

1. **Очень высокая скорость**  
    Gin использует собственный маршрутизатор (роутер), построенный на базе [httprouter](https://github.com/julienschmidt/httprouter).  
    Он обрабатывает тысячи запросов в секунду с минимальными накладными расходами.
    
2. **Middleware (промежуточные обработчики)**  
    Можно легко подключать «прослойки» для логирования, аутентификации, CORS и т.д.
    
3. **Удобный `Context`**  
    В каждом обработчике ты получаешь объект `*gin.Context`, который содержит всё: запрос, ответ, параметры, JSON и даже возможность хранить данные между middleware.
    
4. **Поддержка JSON/XML/YAML/ProtoBuf**  
    Автоматическая сериализация и десериализация.
    
5. **Валидация и биндинг**  
    Можно «привязать» тело запроса (JSON, form, query) к структуре Go и валидировать поля.
    
6. **Группировка маршрутов**  
    Удобно объединять роуты по префиксам и middleware.
    

---

## 🔹 Пример простого API

`package main  import "github.com/gin-gonic/gin"  func main() {     r := gin.Default() // создаём роутер с логированием и recovery middleware      r.GET("/ping", func(c *gin.Context) {         c.JSON(200, gin.H{             "message": "pong",         })     })      r.Run(":8080") // слушаем порт 8080 }`

Запустишь → откроешь `http://localhost:8080/ping` → получишь:

`{"message":"pong"}`

---

## 🔹 Маршруты и параметры

`// Параметры пути r.GET("/user/:name", func(c *gin.Context) {     name := c.Param("name")     c.String(200, "Hello %s", name) })  // Query параметры (?age=20) r.GET("/search", func(c *gin.Context) {     age := c.Query("age")     c.JSON(200, gin.H{"age": age}) })`

---

## 🔹 Работа с JSON

``type Login struct {     User string `json:"user" binding:"required"`     Pass string `json:"pass" binding:"required"` }  r.POST("/login", func(c *gin.Context) {     var json Login     if err := c.ShouldBindJSON(&json); err != nil {         c.JSON(400, gin.H{"error": err.Error()})         return     }     if json.User == "admin" && json.Pass == "1234" {         c.JSON(200, gin.H{"status": "ok"})     } else {         c.JSON(401, gin.H{"status": "unauthorized"})     } })``

---

## 🔹 Middleware пример

`func AuthMiddleware() gin.HandlerFunc {     return func(c *gin.Context) {         token := c.GetHeader("Authorization")         if token != "secret" {             c.AbortWithStatusJSON(401, gin.H{"error": "unauthorized"})             return         }         c.Next()     } }  r := gin.Default() authorized := r.Group("/admin", AuthMiddleware()) authorized.GET("/dashboard", func(c *gin.Context) {     c.JSON(200, gin.H{"status": "ok"}) })`

---

## 🔹 Когда использовать Gin

✅ Если нужно писать **REST API** или микросервисы.  
✅ Если важна **скорость** и простота.  
✅ Если нравится **готовая экосистема** (middleware, плагины, документация).
---

## Related
- [[Study/backend/Go/MOC|Go MOC]]
