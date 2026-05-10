---
tags: [go, echo, web, backend]
---

[**Echo**](https://github.com/labstack/echo) — это **минималистичный, очень быстрый и удобный веб-фреймворк** для Go.  
Главная цель: дать **чистый и простой API**, но при этом оставить высокую производительность.

Echo тоже работает поверх стандартного `net/http`, но с более лёгким и понятным интерфейсом, чем Gin.

---

## 🔹 Особенности Echo

1. **Скорость**  
    Echo по бенчмаркам немного быстрее, чем Gin (но разница не всегда критична).
    
2. **Простое API**  
    Маршруты и обработчики выглядят очень лаконично.
    
3. **Middleware**  
    Встроенные (логирование, CORS, gzip, recover) и возможность писать свои.
    
4. **Работа с JSON**  
    Очень просто: `c.JSON(200, obj)`.
    
5. **Группировка маршрутов**  
    Можно группировать эндпоинты и вешать middleware только на группы.
    
6. **WebSocket, шаблоны и статические файлы**  
    Поддержка из коробки.
    

---

## 🔹 Простой пример Echo

`package main  import (     "net/http"     "github.com/labstack/echo/v4" )  func main() {     e := echo.New()      e.GET("/ping", func(c echo.Context) error {         return c.String(http.StatusOK, "pong")     })      e.Logger.Fatal(e.Start(":8080")) }`

Откроешь `http://localhost:8080/ping` → получишь `pong`.

---

## 🔹 Работа с JSON

``type User struct {     ID   int    `json:"id"`     Name string `json:"name"` }  func main() {     e := echo.New()      e.POST("/users", func(c echo.Context) error {         u := new(User)         if err := c.Bind(u); err != nil {             return err         }         return c.JSON(http.StatusOK, u)     })      e.Logger.Fatal(e.Start(":8080")) }``

Отправишь `POST /users` с JSON:

`{"id":1,"name":"Alice"}`

→ вернётся тот же объект.

---

## 🔹 Middleware пример

`e := echo.New()  // встроенный логгер и восстановление при панике e.Use(middleware.Logger()) e.Use(middleware.Recover())  // кастомный middleware e.Use(func(next echo.HandlerFunc) echo.HandlerFunc {     return func(c echo.Context) error {         if c.Request().Header.Get("X-Token") != "secret" {             return c.JSON(http.StatusUnauthorized, map[string]string{"error": "unauthorized"})         }         return next(c)     } })  e.GET("/secure", func(c echo.Context) error {     return c.String(http.StatusOK, "You are authorized!") })`

---

## 🔹 Сравнение с Gin

|Характеристика|Gin|Echo|
|---|---|---|
|API|Чуть более "магическое"|Простое и лаконичное|
|Скорость|Очень высокая|Ещё чуть выше|
|Экосистема|Огромная|Меньше, но растёт|
|Middleware|Много готовых|Встроенные + простая настройка|
|Популярность|Самый популярный|Второй по популярности
---

## Related
- [[Study/backend/Go/MOC|Go MOC]]
