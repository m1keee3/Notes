
### Названия коммитов

- **add** — Добавил что-то новое (файл, фичу, тест)
    
- **feat** — Добавил новую функциональность или возможность (часто используется вместо `add` для функций проекта)
    
- **fix** — Починил баг или ошибку
    
- **update** — Обновил существующий код, но без кардинальных изменений
    
- **remove** — Удалил файл, код, зависимость и т.д.
    
- **refactor** — Переписал код без изменения поведения (улучшения)
    
- **rename** — Переименовал файл, функцию и т.п.
    
- **improve** — Улучшил читаемость, логику, UI, UX и т.д.
    
- **implement** — Реализовал интерфейс, новую функциональность
    
- **optimize** — Оптимизировал производительность или использование памяти
    
- **reorganize** — Изменил структуру проекта
    
- **test** — Добавил/изменил тесты
    
- **docs** — Добавил или обновил документацию
    
- **init** — Первый коммит, инициализация проекта
    
- **bump** — Обновил версию зависимости или самого проекта
    
- **merge** — Объединил ветки
    
- **revert** — Откатил коммит
    
- **chore** — Вспомогательные изменения, не влияющие на функциональность (например, конфигурация CI/CD, docker-compose, обновление скриптов, настройка среды)

| Тип            | Пример коммита                                                | Когда использовать                                 |
| -------------- | ------------------------------------------------------------- | -------------------------------------------------- |
| **init**       | `init: create project structure`                              | Первый коммит, инициализация проекта               |
| **feat**       | `feat(auth): add JWT login endpoint`                          | Новая функциональность, API, сервис                |
| **fix**        | `fix(cart): correct item quantity calculation`                | Исправление бага                                   |
| **add**        | `add(user): implement basic CRUD operations`                  | Добавление нового файла или теста                  |
| **update**     | `update(config): change default port`                         | Обновление существующего кода без изменений логики |
| **remove**     | `remove(old_service): delete deprecated files`                | Удаление кода, файлов, зависимостей                |
| **refactor**   | `refactor(service): extract helper functions from Run method` | Улучшение структуры кода без изменения поведения   |
| **rename**     | `rename(model): change UserModel to AccountModel`             | Переименование файлов, функций или структур        |
| **improve**    | `improve(logger): add structured logging`                     | Улучшение читаемости, UX, логирования              |
| **implement**  | `implement(service): add Run method with context`             | Реализация интерфейсов, функций                    |
| **optimize**   | `optimize(db): reduce memory usage for queries`               | Оптимизация производительности или памяти          |
| **reorganize** | `reorganize(project): move handlers to separate package`      | Изменение структуры проекта                        |
| **test**       | `test(service): add unit tests for Run method`                | Добавление или изменение тестов                    |
| **docs**       | `docs(readme): add setup instructions`                        | Обновление документации                            |
| **bump**       | `bump(deps): update Go modules`                               | Обновление версий зависимостей                     |
| **merge**      | `merge(feature/auth): integrate login endpoint`               | Объединение веток                                  |
| **revert**     | `revert: fix broken merge from dev`                           | Откат коммита                                      |
| **chore**      | `chore(docker): add docker-compose.yml for services`          | Вспомогательные изменения, конфигурации, скрипты   |