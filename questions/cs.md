# Interview Questions about Programming Theory aka ?Computer Science?

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
Зачем нужно ООП?</summary>

Было решением процедурного программирования

```js
const width = 5
const height = 10

const getArea = (a, b) => a * b

const area = getArea(width, height)
```

процедурное сложно конфигурировать приложение, управлять сущностями + делать декомпозицию

```js
class Rectangle {
	constructor(public width: number, public height: number) {}

	getArea() {
		return this.width * this.height;
	}
	getWidth() {
		return this.width;
	}
	getHeight() {
		return this.height;
	}
}

const rect = new Rectangle(10, 20)
rect.getArea()
rect.getHeight()
```

классовое легко настраивать, добавлять новые методы работы, новые поля

</details>

<details>
<summary>
Пример использования Инкапсуляции ООП
</summary>

### Инкапсуляция (сокрытие)

```js
class Database {
	private _url;
	private _port;
	private _username;
	private _password;
	private _tables;

	constructor(url, port, username, password) {
		this._url = url;
		this._port = port;
		this._username = username;
		this._password = password;
		this._tables = []
	}

	public createTable(table) {
		this._tables.push(table);
	}

	public clearTable() {
		this._tables = [];
	}

	get url() {
		return this._url;
	}
	get port() {
		return this._port;
	}
	get username() {
		return this._username;
	}
	get password() {
		return this._password;
	}
	get tables() {
		return this._tables;
	}
}

const db = new Database("http://localhost", 3000, "admin", "123456");
db.createTable({name: 'roles'})

db.tables = [] // won't work
db.clearTable() // will work
```

поля-настройки скрыты от изменения извне
однако можем их читать и добавлять таблицы через спец методы

</details>

<details>
<summary>
Пример использования Наследования ООП
</summary>

### Наследование

```js
class Person {
	private _name: string
	private _age: number

	constructor(name: string, age: number) {
		this._name = name
		this._age = age
	}

	public get mainInfo() {
		return `im ${this._name} and im ${this._age} yo`
	}

	get name() { return this._name; }
	set name(name: string) { this._name = name; }

	get age() { return this._age; }
	set age(age: number) {
		if(age < 0){
			this._age = 0
		} else {
			this._age = age
		}
	}
}

class Employee extends Person {
	private _inn: string
	private _passport: number

	constructor(name:string, age: number , inn: string, passport: number) {
		super(name, age)
		this._inn = inn
		this._passport = passport
	}

	get inn() { return this._inn; }
	set inn(inn: string) {
		if(isValidInn(inn)) {
			this._inn = inn;
		}
	}

	get passport() { return this._passport; }
	set passport(passport: number) {
		if(isValidPassport(passport)){
			this._passport = passport
		}
	}
}

const emp = new Employee("Temka", 21, "im inn", "im passport")

console.log(emp.inn) // "im inn"
console.log(emp.passport) // "im passport"

class Developer extends Employee {
	private _level: "Junior" | "Middle" | "Senior"
	private _language: string

	constructor(name:string, age: number , inn: string, passport: number, language: string, level: string) {
		super(name, age, inn, passport)
		this._language = language
		this._level = level
	}

	get level() { return this._level}
	set level(level) {
		if(isValidLevel(level)) {
			this._level = level
		}
	}

	get language() { return this._language}
	set language(language) { this._language = language }
}

const developer = new Developer("Temka", 21, "im inn", "im passport", "cobol", "junior")
console.log(developer.mainInfo) // `im Temka and im 21 yo`
```

</details>

<details>
<summary>
Пример использования Полиморфизма ООП
</summary>
### Полиморфизм = поли(много) + морфиус(форма) = много форм

```js
class Person {
	...

	greet() {
		console.log('Hello im person!')
	}
}

class Employee extends Person {
	...

	greet() {
		console.log('Hello im Employee!')
	}
}


class Developer extends Employee {
	...

	greet() {
		console.log('Hello im Developer!')
	}
}


const person = new Person("Temka", 21)
const emp = new Employee("Temka", 21, "im inn", "im passport")
const developer = new Developer("Temka", 21, "im inn", "im passport", "cobol", "junior")

const people: Person[] = [person, emp, developer]

function massGreeting(list: Person[]) {
	for (let i = 0; i < people.length; i++) {
		person.greet()
	}
}
massGreeting(people)
```

</details>
<details>
<summary>
Пример использования Композиция и Аггрегации ООП
</summary>

### Композиция

при композиции, поле создатеся внутри класса

```js
class Car {
	private engine: Engine
	private wheels: Wheel[]

	constructor() {
		this.engine = new Engine()

		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
	}
}
```

Произошла композиция колес и движка в автомобиле

### Агрегация

при агрегации, поле передается через конструктор класса

```js
class Freshener {}

class Car {
	private freshener: Freshener;
	private engine: Engine
	private wheels: Wheel[]

	constructor(freshener) {
		this.freshener = freshener
		this.engine = new Engine()

		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
		this.wheels.push(new Wheel())
	}
}

class LivingRoom {
	freshener: Freshener

	constructor(freshener: Freshener) {
		this.freshener = freshener
	}
}
```

Произошла агреграция освежителя в автомобиль
Произошла агреграция освежителя в квартиру

</details>
<details>
<summary>
Пример использования Интерфейсов и Абстракций (Абстрактный класс) ООП
</summary>

### Interfaces

интерфейсы позволяют писать более гибкий код\
на них лучше проектировать систему

```js
class User {
	name: string
	age: number
}
class Car {
	model: string
	year: number
}

interface Repository<T> {
	create: (obj: T) => void;
	read: () => T;
	update: (obj: T) => void;
	delete: (obj: T) => void;
}

class UserRepository implements Repository<User> {
	create(obj: User) { ... }
	update(obj: User) { ... }
	delete(obj: User) { ... }
	read(): User { ...; return user }
}
class CarRepository implements Repository<Car> {
	create(obj: Car) { ... }
	update(obj: Car) { ... }
	delete(obj: Car) { ... }
	read(): Car { ...; return car }
}
```

### Abstract classes

```js
abstract class AutoFactory {
	readonly name: string;
	readonly model: string;
	year: number

	// must be inited in child class
	abstract getAutoInfo(): string;

	getAutoModel(): string {
		return `модель машины = ${this.model}`;
	}
}

class Auto extends AutoFactory {
	public name: string;
	public model: string;
	public year: number;

	constructor(name: string, model: string, year: number) {
		super()
		this.name = name
		this.model = model
		this.year = year
	}

	getAutoInfo() {
		return `Тип машины = ${this.year} - ${this.model} - ${this.name}`;
	}
}

const audi = new Auto('Audi', 'S Class', 2021)

audi.getAutoInfo()
audi.getAutoModel()
// both work fine

```

</details>
<details>
<summary>
Пример использования Внедрения Зависимостей ООП
</summary>

### Dependency Injection

```js
interface UserRepository {
	getUsers: () => User[];
}

class UserMongoDB {
	getUsers() {
		return [
			{
				name: "John",
				age: 20,
				description: "John from Mongo database",
			},
		]
	}
}
class UserPostgreSQL {
	getUsers() {
		return [
			{
				name: "Temka",
				age: 25,
				description: "Temka from PostgreSQL database",
			},
		]
	}
}

class UserService {
	userRepo: UserRepository

	constructor(userRepo: UserRepository) {
		this.userRepo = userRepo
	}

	getUsersBelowAge(age: number) {
		const users = this.userRepo.getUsers()
		console.log(users)
	}
}

// UserService works fine with any database
// that's called `Dependency injection`
const userService = new UserService(new UserMongoDB())
const userService2 = new UserService(new UserPostgreSQL())

userSerivce.getUsersBelowAge(10)
userService2.getUsersBelowAge(10)
```

</details>

</details>
<details>
<summary>
Пример использования Инверсии Зависимостей ООП
</summary>
	Классы должны зависеть от абстракций, а не от конкретных деталей
</details>

<details>
<summary>
Что такое SOLID (в ООП)?</summary>

SOLID (сокр. от англ. single responsibility, open-closed, Liskov substitution, interface segregation и dependency inversion) = пять основных принципов объектно-ориентированного программирования и проектирования. Принципы SOLID — это руководства, которые также могут применяться во время работы над существующим программным обеспечением для его улучшения - например для удаления «дурно пахнущего кода».

Избавиться от "признаков плохого проекта" помогают следующие пять принципов SOLID:

-   S - Принцип единственной ответственности (The Single Responsibility Principle) каждый класс выполняет лишь одну задачу.
-   O - Принцип открытости/закрытости (The Open Closed Principle) «программные сущности должны быть открыты для расширения, но закрыты для модификации.»
-   L - Принцип подстановки Барбары Лисков (The Liskov Substitution Principle) Наследующий класс должен дополнять, а не изменять базовый.
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

Он соблаюдается при программировании на уровне интерфейсов(создание единого модуля-управлятора для разных API'шек)

Файл который обновляется чаще должен зависеть от файла который зависит реже

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
<summary>
	Что такое паттерн MVC ?
</summary>

View = representation of state

Controller = Actions from View + Actions to Model

Model = = Data + logic

MVC заставляет разделять View и Логику приложения в 2 раздельных слоя = контроллер + модель

,где контроллер = мост между View и Логикой = мост, который передает данные из View в Модель и рендерит данные из модели во View\
Подобное поведение есть в Angular, Backbone

</details>
<details>
<summary>
	Что такое паттерн MVVM?
</summary>

VM вместо Controller

VM = ViewModel = данные приготовленные для отображения = производная от стейта

проблема что viewmodel дает задачи model, а model при обновлении может образовать лавинообразные изменения во View

</details>
