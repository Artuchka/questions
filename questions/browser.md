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
по пути выполнения кода, все действия попадают в call stack (очередь вызовов), кроме асинхронного кода, который исполняетсяо параллельно коду в WebAPI, далее он идёт в callback queue(очередь ожидания), где ждёт своей очереди на вход в call stack

</details>

<details>
<summary>
Какие существуют основные принципы ООП?</summary>

Базовые принципы ООП:

-   Абстракция — отделение концепции от ее экземпляра;
-   Полиморфизм — реализация задач одной и той же идеи разными способами;
-   Наследование — способность объекта или класса базироваться на другом объекте или классе. Это главный механизм для повторного использования кода. Наследственное отношение классов четко определяет их иерархию;
-   Инкапсуляция — размещение одного объекта или класса внутри другого для разграничения доступа к ним.
</details>

<details>
<summary>
Приведите примеры использования основных принципов ООП</summary>

</details>

<details>
<summary>
Что такое SOLID (в ООП)?</summary>

SOLID (сокр. от англ. single responsibility, open-closed, Liskov substitution, interface segregation и dependency inversion) = пять основных принципов объектно-ориентированного программирования и проектирования. Принципы SOLID — это руководства, которые также могут применяться во время работы над существующим программным обеспечением для его улучшения - например для удаления «дурно пахнущего кода».

Избавиться от "признаков плохого проекта" помогают следующие пять принципов SOLID:

-   S - Принцип единственной ответственности (The Single Responsibility Principle) каждый класс выполняет лишь одну задачу.
-   O - Принцип открытости/закрытости (The Open Closed Principle) «программные сущности должны быть открыты для расширения, но закрыты для модификации.»
-   L - Принцип подстановки Барбары Лисков (The Liskov Substitution Principle) «объекты в программе должны быть заменяемыми на экземпляры их подтипов без изменения правильности выполнения программы.» См. также контрактное программирование. Наследующий класс должен дополнять, а не изменять базовый.
-   I - Принцип разделения интерфейса (The Interface Segregation Principle) «много интерфейсов, специально предназначенных для клиентов, лучше, чем один интерфейс общего назначения.»
-   D - Принцип инверсии зависимостей (The Dependency Inversion Principle) «Зависимость на Абстракциях. Нет зависимости на что-то конкретное.»

</details>

<details>
<summary>
Зачем нужны принципы SOLID</summary>

Позволяют разработчикам разговаривать на одном языке\
(на каждом проекте используют свои приниципы, свои фреймворки)\
по полгода объясняют как пишут проект = проектные знания

-   Писать масштабируемые приложения, где легко вносить изменения
-   Порог вхождения снижается
-   Код упрощается
-   Все подходы используют базовые решения и имеют известные ошибки (пример таблицы интегралов)
</details>

<details>
<summary>
Приведите примеры использования принципов SOLID</summary>

#### S: Single responsobility

```js
class Auto {
	model = ''
	year = 1990

	makeCar()
	setCarInfo()

	addCustomerAuto()
	readCustomerAuto()
	updateCustomerAuto()
	deleteCustomerAuto()

	dropAutoDB()
	updateAutoDB()
	addAutoDB()
}
```

Class Auto is overloaded with methods from different areas\
And should be splitted into 3 separate classes

```js
// works with any car
class Auto {
	model = ''
	year = 1990

	makeCar()
	setCarInfo()
}

// works with auto, which customer owns
class CustomerAuto {
	add()
	read()
	update()
	delete()
}

// works with DB
class DB {
	drop()
	update()
	add()
}
```

We make changes in single class, which feature it belongs to

#### O: Opened for new features(class types) \ closed for changing old functionality

The following example makes it hard to add `Toyota` (or any new car brand) to existance\
WITHOUTH CHANGING THE FUNCTIONALITY

```js
for (let i = 0; i < auto.length; i++) {
	switch (auto[i].model) {
		case "Tesla":
			arr.push("80 000 rubles")
		case "Audi":
			arr.push("20 000 dollars")
		default:
			arr.push("no auto price")
	}
}
```

This is bad:
a bunch of if-statements

```js
		case "Toyota":
			arr.push("80 000 rubles")
```

However, if we followed the `O` principle, we should write this:

```js
for (let i = 0; i < auto.length; i++) {
	arr.push(auto[i].getCarPrice())
}
```

ofc, we'll have to create the method for each new Auto brand since now\
tho we can leave old functionality working as expected (it's already tested & works)

#### L: Liskov substitution

imagine having Rectangle

```js
class Rectangle {
	constructor(public width: number, public height: number) {}

	setWidth(width: number) {
		this.width = width
	}
	setHeight(height: number) {
		this.height = height
	}

	areaOf() {
		return this.width * this.height
	}
}
```

now you want to have a Square (why not crete it from Rectangle, right...?)

```js
class Square extends Rectangle {
	constructor(width: number) {
		super(width, width)
	}

	setWidth(width: number) {
		this.width = width
		this.height = width
	}
	setHeight(height: number) {
		this.height = height
		this.width = height
	}
}
```

yeah, we can change square sides\
but imagine we'd work in some function with `Rectangle` instance\
BUT actually it was `Square`, so the function will think it works with `Rectangle`, tho it's not...

```js
const changeSizes = (figure: Rectangle) => {
	figure.setWidth(10)
	figure.setHeight(20) // at this point function expects figure to have width=10 and height=20
}
// hmm u see, yeah?
// this function works differently for Rectangle and Square
```

```js
interface Figure {
	setWidth(value: number): void;
	setHeight(value: number): void;
	areaOf(): void;
}

class Rectangle extends Figure {
	setWidth(value: number) { ... }
	setHeight(value: number) { ... }
	areaOf() { ... }
}

class Square extends Figure {
	setWidth(value: number) { ... }
	setHeight(value: number) { ... }
	areaOf() { ... }
}
```

now it's easier to distinguish between square and rectangle

##### Second example for Liskov

```js
class Database {
	connect() {}
	read() {}
	write() {}
	joinTables() {}
}


class MySQLDatabase extends Database {
	connect() {}
	read() {}
	write() {}
	joinTables() {}
}
class MongoDB extends Database {
	connect() {}
	read() {}
	write() {}
	// u see yeah?
	// child class `MongoDB` breaks logic from parent class `Database`
	// Liskov principle violated
	joinTables() {
		throw new Error("MongoDB has no support for tables")
	}
```

```js
class Database {
	connect() {}
	read() {}
	write() {}
}

class SQLDatabase {
	connect() {}
	read() {}
	write() {}
	joinTables() {}
}

class NoSQLDatabase {
	connect() {}
	read() {}
	write() {}
	createIndex() {}
}
// now that's good!! Liskov is happy
class MySQLDatabase extends SQLDatabase { ... }
class MongoDB extends NoSQLDatabase { ... }
```

#### I: Interface Segragation

lmao this one is funny

```js
interface Weapon {
	attack(): void;
	reload(): void;
}

interface GlockNine extends Weapon {}
interface RPG extends Weapon {}

interface Knife extends Weapon {}
// BUT wait a second, knife does not need reloading...
```

so we should segregate the weapon methods into separate interfaces

```js
// names are shitty, i know
interface AbleToAttack {
	attack(): void;
}
interface AbleToReload {
	reload(): void;
}

// NICE !!
// It's so easy to create new weapons now
interface GlockNine extends AbleToAttack, AbleToReload {}
interface RPG extends AbleToAttack, AbleToReload {}
interface Knife extends AbleToAttack {}
```

#### D: Dependency Inversion

Upper modules should not depend on lower modules
abstraction

imagine we have music app, which gets songs from Yandex\
=> `getSongs` is the method which does it

```js
class YandexMusicApi {
	getSongs() {}
}

const MusiApp = () => {
	const API = new YandexMusicApi()

	const songs = API.getSongs()
}
```

now we are tired of Yandex, we want Spotify

```js
class YandexMusicApi {
	getSongs() {}
}

class SpotifyMusicApi {
	findAllSongs() {}
}

const MusiApp = () => {
	// clearly, we must change it here
	const API = new SpotifyMusicApi()

	// but may we leave it as it was?
	// because each api has lots of different named methods
	const songs = API.findAllSongs()
}
```

so it would be better to create MusicClient

```js
interface MusicApi {
	getTracks: () => void;
}

class YandexMusicApi implements MusicApi {
	getTracks(): void {}
}
class SpotifyMusicApi implements MusicApi {
	getTracks(): void {}
}
class VKMusicApi implements MusicApi {
	getTracks(): void {}
}

const MusiApp = () => {
	// 1. clearly, we must change it here
	const API: MusicApi = new SpotifyMusicApi()
	// 3. tho what if we have lots of api instances for spotify
	// 4. and now willing to have Yandex instead

	// 2. but this always stays the same
	const songs = API.getTracks()
}
```

let add client which helps us to abstract the APIs

```js
class MusicClient implements MusicApi()  {
	client: MusicApi;
	constructor(public client: MusicApi) {
		// this.client = client;
	}

	getTracks() {
		this.client.getTracks();
	}
}

const MusiApp = () => {
	const API: MusicApi = new MusicClient(new SpotifyMusicApi())

	const songs = API.getTracks()
}
```

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
