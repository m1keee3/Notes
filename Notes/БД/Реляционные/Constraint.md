
**Constraint (Ограничение)** — это правило, enforced (принудительно применяемое) системой управления базами данных (СУБД) для обеспечения целостности и непротиворечивости данных в таблицах.

## Типы constraint
#### 1. NOT NULL — запрет пустых значений

```sql
CREATE TABLE Employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,  -- Имя обязательно
    email VARCHAR(100) NOT NULL  -- Email обязателен
);
```

**Что проверяет:** Столбец не может содержать NULL значения

#### 2. UNIQUE — гарантия уникальности


```sql
CREATE TABLE Users (
    id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,  -- Логин должен быть уникальным
    email VARCHAR(100) UNIQUE     -- Email должен быть уникальным
);
```


**Что проверяет:** Все значения в столбце должны быть разными (кроме NULL)

#### 3. PRIMARY KEY — первичный ключ


```sql
CREATE TABLE Products (
    product_id INT PRIMARY KEY,  -- Уникальный идентификатор товара
    name VARCHAR(100)
);
```


**Что проверяет:** Комбинация NOT NULL + UNIQUE для идентификации записей

#### 4. FOREIGN KEY — внешний ключ

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(id)  -- Ссылка на существующего пользователя
);
```

**Что проверяет:** Значение должно существовать в связанной таблице

#### 5. CHECK — пользовательская проверка

```sql
CREATE TABLE Students (
    id INT PRIMARY KEY,
    age INT CHECK (age >= 16 AND age <= 100),  -- Возраст от 16 до 100
    grade CHAR(1) CHECK (grade IN ('A', 'B', 'C', 'D', 'F'))  -- Допустимые оценки
);
```

**Что проверяет:** Произвольное условие, которое должно быть истинно

#### 6. DEFAULT — значение по умолчанию

```sql
CREATE TABLE Tasks (
    id INT PRIMARY KEY,
    title VARCHAR(200),
    created_date DATE DEFAULT CURRENT_DATE,  -- Дата создания по умолчанию
    status VARCHAR(20) DEFAULT 'pending'     -- Статус по умолчанию
);
```

**Что обеспечивает:** Автоматическое заполнение значения при вставке