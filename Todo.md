
```todoist  
filter: "today | overdue"
name: "🔥 Сегодня / Просрочено"
sorting:
    - priorityDescending
    - date
groupBy: priority
view:
  noTasksMessage: "🎉 На сегодня нет задач, можно и отдохнуть!" 
```

```todoist
filter: "##Yawork"
name: "Yawork"
groupBy: project
sorting:
    - priorityDescending
    - date
show:
    - due
    - labels
view:
  noTasksMessage: "✨ Тут пусто"
```

```todoist
filter: "#personal"
name: "🏠 Personal"
sorting:
    - priorityDescending
    - date
show:
    - due
    - labels
view:
  noTasksMessage: "✨ Тут пусто"
```

```todoist
filter: "##Projects"
name: "📊 Projects"
groupBy: project
sorting:
    - priorityDescending
    - date
show:
    - due
    - labels
view:
  noTasksMessage: "✨ Тут пусто"
```
