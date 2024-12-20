# Подключение API AdGrid

## Типы

```ts
telegramId: number // id игрока который получает таски
```

```ts
taskId: string // uuid задания
```

```ts
type Task = {
  ID: string;
  name: string;
  link: string;
  bonus: string;
}
```

## Получение списка тасок

```ts
async function getTasks(telegramId: number) {
  try {
    const req = await fetch(`https://hubforad-test.printer-game.com/api/tasks?player_id=${telegramId}`);

    const res = await req.json();
    if (!req.status) throw new Error(`${res.error}`);

    return res.data as Array<Task>
  } catch (error) {
    console.error(`${(error as Error).name}: ${(error as Error).message}`);
    return [] as Array<Tasl>;
  }
}
```

Данная функция вернёт список из задач

Вот пример выхода:

```json
[
  {
    "ID": "a2534ccc-83aa-4eda-b42e-7106491fdbac",
    "bonus": "99999",
    "link": "t.me",
    "name": "Открой ТГ в браузере"
  },
  {
    "ID": "c294a5e2-02fa-4a4d-ab0a-d3228cd9528e",
    "bonus": "99999",
    "link": "http://google.com",
    "name": "Открой гугл в браузере"
  }
]
```

## Запрос на проверку таски

```ts
async function checkTask(telegramId: number, taskId: string) {\
    try {
        const req = await fetch(`https://hubforad-test.printer-game.com/tasks/complete`, {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({
                player_id: telegramId,
                task_id: taskId,
            }),
        });
        const res = await req.json();
        if (res?.message !== 'task completed successfully') {
            if (!res.status) throw new Error(`${res.error}`);
        }
    } catch (error) {
        console.error(`${(error as Error).name}: something went wrong - ${(error as Error).message}`);
    }
}
```

# Что нужно от вашего API

## GET метод для применения награды

Что принимает запрос через url параметры: 
```
telegramId - id игрока
```

Пример ссылки

`
https://example.com:3000/admin/add-to-task-balance?telegramId=%s
`

`%s` здесь указывается в том месте куда нужно подставить id игрока

Так же метод может принимать какие либо мета данные, их нужно указать в ссылке, т.к. `API AdGrid` меняет только `%s` 
