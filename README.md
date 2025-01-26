# Table of Contents


- [Connecting the Hubforad API](#Connecting-the-Hubforad-API)
  - [Types](#Types)
  - [Getting the list of tasks](#Getting-the-list-of-tasks)
  - [Request for verification of task completion](#Request-for-verification-of-task-completion)
- [What we need from your API](#What-we-need-from-your-API)
  - [GET method to apply the award](#GET-method-to-apply-the-award)

# Roadmap
- [Add a method to get tasks](#Getting-the-list-of-tasks)
- [Add a method to check the task](#Request-for-verification-of-task-completion)
- [Add a method to reward users](#GET-method-to-apply-the-award)
- Send this method to chat with the Hubforad team

# Connecting the Hubforad API

## Types

```ts
telegramId: number // id of the player to whom the tasks will be shown
```

```ts
taskId: string // id of the task
```

```ts
type Task = {
  ID: string;
  name: string;
  link: string;
  bonus: string;
  icon_link: string;
}
```

## Getting the list of tasks

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

This function will return a list of tasks

Here's an example of output:

```json
[
  {
    "ID": "a2534ccc-83aa-4eda-b42e-7106491fdbac",
    "bonus": "99999",
    "link": "t.me",
    "name": "Open Telegram in your browser",
    "icon_link": "https://hubforad-test.printer-game.com/api/s3/81f47cf4-c0ce-44c1-8c25-d0c5e25b77e7.jpeg"
  },
  {
    "ID": "c294a5e2-02fa-4a4d-ab0a-d3228cd9528e",
    "bonus": "99999",
    "link": "http://google.com",
    "name": "Open Google in your browser",
    "icon_link": "https://hubforad-test.printer-game.com/api/s3/fea4f08a-5ccf-45b4-9d4f-3253e6b9c1a0.jpeg"
  }
]
```

## Request for verification of task completion

```ts
async function checkTask(telegramId: number, taskId: string) {\
    try {
        const req = await fetch(`https://hubforad-test.printer-game.com/api/tasks/complete`, {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: JSON.stringify({
                player_id: telegramId,
                task_id: taskId,
                block_id: "bbd1eb06-8bcc-4d1a-b383-e242d9938808"
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

bbd1eb06-8bcc-4d1a-b383-e242d9938808 - that's your unique id that the ([admin](https://t.me/ray6right)) will give you

# What we need from your API

## GET method to apply the award

What accepts a request via url parameters: 
```
telegramId
```

Example of a link

`
https://example.com:3000/admin/add-to-task-balance?telegramId=%s
`

`%s` here is the place where the player id should be inserted

Also the method can accept any meta data, it must be specified in the reference, because `API Hubforad` changes only `%s`
