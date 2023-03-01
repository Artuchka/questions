<details>
<summary>
Что такое HTTP?
</summary>

Протокол прикладного уровня для передачи данных по сети\
Самый частоиспользуемый

</details>

<details>
<summary>
Из чего состоит HTTP запрос?
</summary>

-   Строка запроса RequestLine (method, url, http version)
-   Заголовки Message Header (описывают тело сообщения, передача параметров)
-   тело сообщения, entity body (сама информация, может отсутсвовать)

</details>

<details>
<summary>
Какие методы HTTP есть и для чего они?
</summary>

частоиспользуемые:

-   GET - для получения данных, нет entity body
-   POST - для создания данных, есть entity body
-   DELETE - для удаления данных, есть entity body
-   PATCH- для изменения данных, есть entity body
-   PUT - для перезаписи существущих данных, есть entity body
-   HEAD -
-   COPY -

Ничего не мешает удалять данные POST запросом, но для лучше семантики стоит использовать DELETE\
поглядите Microsoft Giudelines для построения rest api

</details>

<details>
<summary>
Что такое WebSocket?
</summary>

upgraded http протокол

протокол для взаимодействия в реальном времени(real-time)

сообщения передаются пока одна из сторон не закроет соединение

</details>

<details>
<summary>
Что такое REST API?
</summary>

популярный архитектурный подход клиент-сервер

разделение CRUD операций к одному и тому же URL с помощью HTTP методов

</details>

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
