# Interview Questions about Web

<details>
<summary>
Что такое HTTP?
</summary>
Hypertext Transfer Protocol

Протокол прикладного уровня для передачи данных по сети

Самый частоиспользуемый

Он создан для связи между веб-браузерами и веб-серверами, хотя в принципе HTTP может использоваться и для других целей. Протокол следует классической клиент-серверной модели, когда клиент открывает соединение для создания запроса, а затем ждет ответа. HTTP - это протокол без сохранения состояния, то есть сервер не сохраняет никаких данных (состояние) между двумя парами "запрос-ответ". Несмотря на то, что HTTP основан на TCP/IP, он также может использовать любой другой протокол транспортного уровня с гарантированной доставкой.

Ниже перечислены общие функции, управляемые с HTTP:

-   Кэш. Сервер может инструктировать прокси и клиенты: что и как долго кэшировать. Клиент может инструктировать прокси промежуточных кэшей игнорировать хранимые документы.

-   Ослабление ограничений источника. Для предотвращения шпионских и других, нарушающих приватность, вторжений, веб-браузер обчеспечивает строгое разделеление между веб-сайтами. Только страницы из того же источника могут получить доступ к информации на веб-странице. Хотя такие ограничение нагружают сервер, заголовки HTTP могут ослабить строгое разделение на стороне сервера, позволяя документу стать частью информации с различных доменов (по причинам безопасности).

-   Аутентификация. Некоторые страницы доступны только специальным пользователям. Базовая аутентификация может предоставляться через HTTP, либо через использование заголовка WWW-Authenticate и подобных ему, либо с помощью настройки спецсессии, используя куки.

-   Прокси и тунелирование. Серверы и/или клиенты часто располагаются в интранете, и скрывают свои истинные IP-адреса от других. HTTP запросы идут через прокси для пересечения этого сетевого барьера. Не все прокси -- HTTP прокси. SOCKS-протокол, например, оперирует на более низком уровне. Другие, как, например, ftp, могут быть обработаны этими прокси.

-   Сессии. Использование HTTP кук позволяет связать запрос с состоянием на сервере. Это создает сессию, хотя ядро HTTP -- протокол без состояния. Это полезно не только для корзин в интернет-магазинах, но также для любых сайтов, позволяющих пользователю настроить выход.

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
-   PATCH- для частичного изменения данных, есть entity body
-   PUT - для перезаписи\замены существущих данных, есть entity body
-   OPTIONS - используется для описания параметров соединения с ресурсом.
-   HEAD - запрос данных как GET, но без тела ответа

HEAD For example, if a URL might produce a large download, a HEAD request could read its Content-Length header to check the filesize without actually downloading the file.

OPTIONS To find out which request methods a server supports\
In CORS, a preflight request is sent with the OPTIONS method so that the server can respond if it is acceptable to send the request. In this example, we will request permission for these parameters\
[options by dev mozilla](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/OPTIONS)

```js
//req
curl -X OPTIONS https://example.org -i

//res
HTTP/1.1 204 No Content
Allow: OPTIONS, GET, HEAD, POST
Cache-Control: max-age=604800
Date: Thu, 13 Oct 2016 11:45:00 GMT
Server: EOS (lax004/2813)
```

Ничего не мешает удалять данные POST запросом, но для лучше семантики стоит использовать DELETE\
поглядите Microsoft Giudelines для построения rest api

</details>

<details>
<summary>
Какие заголовки HTTP для кеширования?
</summary>

-   Cache-Control = управляет кешировать ли + если да, то где и как

```html
Cache-Control: no-cache, no-store, must-revalidate Cache-Control: private
Cache-Control: public
```

-   Expiration
    директива "max-age=<seconds>" — максимальное время, в течение которого ресурс считается "свежим"

-   Pragma

</details>

<details>
<summary>
Какие заголовки Cookie для безопасности?
</summary>

-   SameSite: None \ Lax \ Strict \ None
-   secure: frue \ false
-   httpOnly: frue \ false

одинаковые домены но разными схемами http и https = разыне сайты для кукисов

#### SameSite определяет надо ли блокировать куки для третьих лиц

-   Strict = Кукисы будут отправлены только для запросов со своего оригина, но не для запросов с сайтов другого домена

    подходит для кукисов, позволяющих:

    -   изменять пароль на сайте
    -   совершать покупку
    -   выпускать ядерные ракеты и т.д.

-   Lax = кукисы на другой домен не отправляются + из-за чужого ресурса(картинка с твоего блога) тоже нет, но если юзер пойдет на сайт кукисов(на твой блог), то отправятся

    pov: ты залогинен на ютюбе\
    и зашел на чужой сайт\
    видос придложится сохранить "Посмотреть позже" именно в твой аккаунт\
    т.к. у тебя есть Lax кукисы\
    => с сайта pereborom.ru отсылается запрос на youtube.com добавляющий этот видос в "Посмотреть позже"

    pov: ты очистил кукисы от ютюба и local storage от ютюба\
    => _ ты будто вышел из аккаунта гугловского (на самом деле мануально вышел) _\
    => на сайте у тебя не будет фичи добавить видос в "Посмотреть позже"\
    пока ты не залогинишься на ютюбчике снова

-   None = Кукисы отсылаются с любого домена, НО Только Secure

#### Secure

защищает кукисы от кражи на незащищенных соединениях, т.к. secure=true разрешает отправлять кукисы только на HTTPS соединениях

#### HttpOnly

HttpOnly=true

запрещает доступ JavaScript'у доступ к кукисам (через document.cookie)\
но разрешает отсылать их в запросах

Если указано withCredentials=true, то любые кукисы(и даже HttpOnly=false) будут отсылаться, но не будут видны в любом случае

</details>

<details>
<summary>
Что такое Cross-site request forgery (CSRF) атака?
</summary>

атака пользуется тем, что кукисы прикрепляются к любому запросу на данном домене в независимости кто их отправляет

```html
<!-- Картинка на сайте evil.example -->
<img src="https://www.youtube.com/index.php?action=delete&id=123" />
```

например, посещаем сайт evil.example\
тогда он может послать запрос, например, на `удаление видоса` на youtube.com (ведь в примере с "Watch Later" смог)\
если youtube.com не проверит такой запрос, то пиши пропало, беда пришла всему западу, лопнет как мыльный пузырь наш IT-толстячок.. хеххе....

по-русски = "Подложный запрос с чужого домена"

для защиты:

-   RESTful API
-   secure tokens

</details>

<details>
<summary>
Что такое WebSocket? Приницпы работы с ним?
</summary>

upgraded http протокол

протокол для взаимодействия в реальном времени(real-time)

сообщения передаются пока одна из сторон не закроет соединение

Протокол WebSocket («веб-сокет»), описанный в спецификации RFC 6455, обеспечивает возможность обмена данными между браузером и сервером через постоянное соединение. Данные передаются по нему в обоих направлениях в виде «пакетов», без разрыва соединения и дополнительных HTTP-запросов.

Чтобы открыть веб-сокет-соединение, нам нужно создать объект new WebSocket, указав в url-адресе специальный протокол ws:

```js
let socket = new WebSocket("ws://javascript.info")
```

Как только объект WebSocket создан, мы должны слушать его события. Их всего 4:

-   open – соединение установлено,
-   message – получены данные,
-   error – ошибка,
-   close – соединение закрыто.

Вот пример:

```js
let socket = new WebSocket("wss://javascript.info/article/websocket/demo/hello")

socket.onopen = function (e) {
	alert("[open] Соединение установлено")
	alert("Отправляем данные на сервер")
	socket.send("Меня зовут Джон")
}

socket.onmessage = function (event) {
	alert(`[message] Данные получены с сервера: ${event.data}`)
}

socket.onclose = function (event) {
	if (event.wasClean) {
		alert(
			`[close] Соединение закрыто чисто, код=${event.code} причина=${event.reason}`
		)
	} else {
		// например, сервер убил процесс или сеть недоступна
		// обычно в этом случае event.code 1006
		alert("[close] Соединение прервано")
	}
}

socket.onerror = function (error) {
	alert(`[error] ${error.message}`)
}
```

Вызов socket.send(body) принимает body в виде строки или любом бинарном формате включая Blob, ArrayBuffer и другие. Дополнительных настроек не требуется, просто отправляем в любом формате. При получении данных, текст всегда поступает в виде строки. А для бинарных данных мы можем выбрать один из двух форматов: Blob или ArrayBuffer.

</details>

<details>

<summary>
	Что такое Cross-Origin Resource Sharing (CORS)?
</summary>

Cross-Origin Resource Sharing (CORS) — механизм, использующий дополнительные HTTP-заголовки, чтобы дать возможность агенту пользователя получать разрешения на доступ к выбранным ресурсам с сервера на источнике (домене), отличном от того, что сайт использует в данный момент. Говорят, что агент пользователя делает запрос с другого источника (cross-origin HTTP request), если источник текущего документа отличается от запрашиваемого ресурса доменом, протоколом или портом.

В целях безопасности браузеры ограничивают cross-origin запросы, инициируемые скриптами. Например, XMLHttpRequest и Fetch API следуют политике одного источника (same-origin policy). Это значит, что web-приложения, использующие такие API, могут запрашивать HTTP-ресурсы только с того домена, с которого были загружены, пока не будут использованы CORS-заголовки.

</details>

<details>
<summary>
Что такое RESTful API?
</summary>

REST = стиль архитектуры программы

RESTful API = это HTTP API, которые можно считать RESTful, если они придерживаются ограничений REST архитектуры

популярный архитектурный подход клиент-сервер

ограничения:

-   для передачи данных обычно JSON
-   разделение CRUD операций к одному и тому же URL с помощью HTTP методов
-   клиент-серверный протокол + без состояния + может кешировать

REST = Representaional State Transfer = Представительная Передача Состояния

</details>

<details>
<summary>
Как можно хранить данные в бразуре? Разница способов?
</summary>

Cookies, Local Storage, Session Storage

1. cookies:

    - отправляются вместе с каждым HTTP запросом
    - можно указать время самоуничтожения (expearation time)
    - есть ограничение по памяти
    - могут быть доступны для других сайтов

2. session storage & local storage:

    - данные хранятся пока их не удалят явным образом
    - данные доступны ТОЛЬКО для того же самого origin'а
    - сервер не может их изменять как кукисы

3. session storage:

    - данные доступны только внтури browser tab
    - данные удаляются при закрытии browser tab

4. local storage:

    - данные НЕ удаляются при закрытии browser tab или браузера

5. indexDB:

    - для больших проектов в браузерах Chromium

```html

```

</details>

<details>
<summary>
Что такое HTTP cookie и для чего их используют?
</summary>

HTTP cookie (web cookie, cookie браузера) - это небольшой фрагмент данных, отправляемый сервером на браузер пользователя, который тот может сохранить и отсылать обратно с новым запросом к данному серверу. Это, в частности, позволяет узнать, с одного ли браузера пришли оба запроса (например, для аутентификации пользователя). Они запоминают информацию о состоянии для протокола HTTP, который сам по себе этого делать не умеет.

Cookie используются, главным образом, для:

-   Управления сеансом (логины, корзины для виртуальных покупок)
-   Персонализации (пользовательские предпочтения)
-   Мониторинга (отслеживания поведения пользователя)

Получив HTTP-запрос, вместе с откликом сервер может отправить заголовок Set-Cookie с ответом. Cookie обычно запоминаются браузером и посылаются в значении заголовка HTTP Cookie с каждым новым запросом к одному и тому же серверу. Можно задать срок действия cookie, а также срок его жизни, после которого cookie не будет отправляться. Также можно указать ограничения на путь и домен, то есть указать, в течении какого времени и к какому сайту оно отсылается.

Куки можно создавать через JavaScript при помощи свойства Document.cookie. Если флаг HttpOnly не установлен, то и доступ к существующим cookies можно получить через JavaScript.

```js
document.cookie = "yummy_cookie=choco"
document.cookie = "tasty_cookie=strawberry"
```

</details>

<details>
<summary>
Что такое цикл событий (event loop) и как он работает?</summary>

event loop является малой частью в большом механизме, который организует "ассинхронность" в браузере(или другом js runtime)

код в джс читается и выполняется последовательно сверху вниз
по пути выполнения кода:

1. все действия попадают в call stack (очередь вызовов)
2. кроме асинхронного кода, который исполняетсяо параллельно коду в WebAPI
3. далее асинхронный код идёт в callback queue(очередь ожидания)
4. асинхронный код ждёт своей очереди на вход в call stack
5. когда полностью освобождается call stack, то наш асинхронный код выполняется в call stack'е

</details>

<details>
<summary>Что такое Веб-компоненты и какие технологии в них используются?</summary>

Веб-компоненты — технология, которая позволяет создавать многократно используемые компоненты в веб-документах и веб-приложениях. Веб-компоненты поддерживаются веб-браузерами напрямую и не требуют дополнительных библиотек для работы.

Веб-компоненты включают четыре технологии, каждая из которых может использоваться отдельно от других:

-   Custom Elements — API для создания собственных HTML элементов.
-   HTML Templates — тег позволяет реализовывать изолированные DOM-элементы.
-   Shadow DOM — изолирует DOM и стили в разных элементах.
-   HTML Imports — импорт HTML документов.
</details>

<details>
<summary>Как уменьшить время загрузки страницы?</summary>

-   разделять код на чанки (chunks)
-   сжимать картинки + выдавать соответсвующие для viewport'а
-   lazy loading
-   ServerSideRendering

-   service worker ИЛИ Web Worker для больших вычислений
-   content delivery network = географически распределенная сетевая система серваков, выдающих сайт ближайщему к себе юзеру
-   кеширование сайта
-   кеширование на уровне серверов = нижний сервер посылает запрос верхнему

</details>
<details>
<summary>Плюсы ServerSideRendering?</summary>

-   хорош для SEO = роботы читают готовый сайт, а не грузят его джс
-   повышает скорость загрузки страницы

</details>
