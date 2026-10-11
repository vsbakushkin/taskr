# taskr

Небольшой REST API для задач на Rust и [Axum](https://github.com/tokio-rs/axum). Задачи хранятся в памяти, поэтому при каждом перезапуске список сбрасывается.

Это учебный проект, сделанный по видео [Learn Axum in 2026: Rust REST API for Beginners](https://youtu.be/iPNEhKMIqNg) с канала Codynn.

## Запуск

```sh
cargo run
```

Сервер слушает `http://localhost:3000` и стартует с тремя заранее добавленными задачами.

## API

| Метод    | Путь          | Описание                       | Успех       |
| -------- | ------------- | ------------------------------ | ----------- |
| `GET`    | `/health`     | Проверка работоспособности     | `200`, `Ok` |
| `GET`    | `/tasks`      | Список всех задач              | `200`       |
| `POST`   | `/tasks`      | Создать задачу                 | `201`       |
| `GET`    | `/tasks/{id}` | Получить одну задачу           | `200`       |
| `PATCH`  | `/tasks/{id}` | Изменить `title` и/или `done`  | `200`       |
| `DELETE` | `/tasks/{id}` | Удалить задачу                 | `204`       |

Запросы с неизвестным `id` возвращают `404`.

Задача выглядит так:

```json
{ "id": 1, "title": "Learn Axum", "done": false }
```

## Примеры

```sh
curl localhost:3000/tasks

curl -X POST localhost:3000/tasks \
  -H 'Content-Type: application/json' \
  -d '{"title": "Write a README"}'

curl -X PATCH localhost:3000/tasks/1 \
  -H 'Content-Type: application/json' \
  -d '{"done": true}'

curl -X DELETE localhost:3000/tasks/2
```

## Структура

```
src/
  main.rs            точка входа, привязка к порту
  routes.rs          роутер и таблица маршрутов
  state.rs           общее состояние в памяти и начальные данные
  models.rs          Task и тела запросов
  handlers/tasks.rs  CRUD-обработчики
```
