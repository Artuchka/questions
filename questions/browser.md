<details>
<summary>
Как можно хранить данные в бразуре? Разница способов?
</summary>

Cookies, Local Storage, Session Storage

cookies:

-   отправляются вместе с каждым HTTP запросом
-   можно указать время самоуничтожения (expearation time)

session storage:

-   данные доступны только внтури browser tab
-   данные доступны только для того же самого origin'а
-   данные хранятся пока их не удалят явным образом
-   данные удаляются при закрытии browser tab

local storage:

-   данные доступны только для того же самого origin'а
-   данные НЕ удаляются при закрытии browser tab или браузера
-   данные хранятся пока их не удалят явным образом
-   есть ограничение по памяти

```html

```

</details>