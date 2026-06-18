# Newman API Tests

Автоматизированные API-тесты для [Platzi Fake Store API](https://api.escuelajs.co/api/v1), запускаемые через [Newman](https://github.com/postmanlabs/newman) (CLI-раннер Postman) и интегрированные в CI/CD на GitHub Actions.

## Что тестируется

Коллекция `plati-store.postman.json` содержит запросы и проверки (`pm.test`) для основных сценариев работы магазина:

- **Authorization** — получение access/refresh токенов (`getToken`), профиль пользователя (`getUserProfile`)
- **Users** — создание (`createUser`) и удаление (`deleteUser`) пользователя
- **Filter Products** — фильтрация товаров по названию, цене и диапазону цен
- **Products** — получение списка товаров и товара по ID

Токен авторизации сохраняется в переменную коллекции и переиспользуется в защищённых запросах.

## Структура проекта

```
.
├── plati-store.postman.json     # Postman-коллекция с запросами и тестами
├── .github/workflows/main.yaml  # CI: запуск тестов и публикация отчёта
├── newman/                      # HTML-отчёты (генерируются, в .gitignore)
└── package.json
```

## Запуск локально

Установите зависимости:

```bash
npm install -g newman newman-reporter-htmlextra
```

Запустите тесты с HTML-отчётом:

```bash
newman run plati-store.postman.json -r htmlextra --reporter-htmlextra-export newman/index.html
```

## CI/CD

Workflow `newman-api-tests` запускается при push в `main`/`master` и вручную (`workflow_dispatch`):

1. Поднимает Node.js 22.14.0 и устанавливает Newman + htmlextra-репортер.
2. Прогоняет коллекцию и генерирует HTML-отчёт в `newman/`.
3. Загружает отчёт как артефакт (хранится 7 дней).
4. Публикует отчёт на GitHub Pages (ветка `gh-pages`).

Отчёт доступен после прогона на странице GitHub Pages репозитория.
