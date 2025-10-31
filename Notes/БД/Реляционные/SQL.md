### Синтаксис `SELECT`

Рассмотрим запрос выборки в SQL - его можно разделить на 5 частей:

1. **Выбор столбцов отношения**:
    
```
 SELECT [ DISTINCT | ALL ] { * | [ColumnExpression [AS NewName] ] [, ...]}
```
    
Ключевое слово `DISTINCT` определяет выбор только уникальных кортежей, `ALL` - явный выбор кортежей с дубликатами
	
Далее указываются имена столбцов (также возможны их алиасы) или `*`, которая определяет вывод всех столбцов отношения
    
2. **Выбор исходного отношения**:
    
```
 FROM TableName [AS NewTableName] 
 [{INNER | LEFT OUTER | FULL} JOIN OuterTable [AS NewOuterTableName] 
 ON Condition]
```
    
Здесь же можно определить соединение и его тип на основе условия `Condition`
    
3. **Фильтрация кортежей**:
    
```
 [WHERE Condition]
```
    
В инструкции `WHERE` определяются условия для фильтрации кортежей
    
4. **Группировка**:
    
```
 [GROUP BY ColumnList [, ...] [HAVING Condition]]
```
    
В инструкции `GROUP BY` производится группировка по указанному набору атрибутов и фильтрации через условие в `HAVING`
    
5. **Сортировка**:
    
```
 [ORDER BY ColumnList [, ...] [{ASC | DESC}]]
```
    
И, наконец, в инструкции `ORDER BY` происходит сортировка конечного отношения по указанному набору атрибутов
    

В конечном счете, получаем:

```
SELECT [ DISTINCT | ALL ] { * | [ColumnExpression [AS NewName] ] [, ...]}
FROM TableName [AS NewTableName] 
[{INNER | LEFT OUTER | FULL} JOIN OuterTable [AS NewOuterTableName] 
ON Condition]
[WHERE Condition]
[GROUP BY ColumnList [, ...] [HAVING Condition]]
[ORDER BY ColumnList [, ...] [{ASC | DESC}]]
```

Здесь стоит заметить, что желательно общие условия, которые имеют место в инструкции `WHERE` стоит размещать именно там, а не в `HAVING`, так как фильтрация кортежей после группировки работает медленнее и не обеспечивает производительность

Порядок выполнения инструкций в `SELECT` запросе таков:

1. `FROM`
2. `ON`
3. `JOIN`
4. `WHERE`
5. `GROUP BY`
6. `HAVING`
7. `SELECT`
8. `DISTINCT`
9. `ORDER BY`

---

## Join

