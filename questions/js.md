# Interview Questions about JavaScript

<details>
<summary>Какие типы данных существуют в JS?</summary>
- Number
- String
- Boolean
- null
- undefined
- Bigint
- Object
- Symbol

```js
num = 101
str = "101"
boolean = true
n = null // ничего, пусто
undef = undefined // переменная после объявления, без инициализации
bigintNumber = 132212387123n // для больших чисел
obj = { name: "temka" }
symb = new Symbol("some string; я возвращаю уникальную ссылку")
```

</details>

<details>
<summary>Что такое this?</summary>

ключевое слово котороые ссылается на контекст\
в глобальной обласи видимости this = window в браузере\
globalThis в nodejs

```js
const user = {
	age: 1000,
	getAgeDeclaration() {
		return this.age
	},
	getAgeExpression: () => {
		return this.age
	},
}

console.log(user.getAgeDeclaration()) // 1000
console.log(user.getAgeExpression()) // undefined
```

</details>

<details>
<summary>Какие есть способы обхода массивов? Их особенности</summary>

### while() do{}

сначала проверятся условие, потом делается итерация

### do() while{}

первая итерация будет в любом случае, проверятся условие для следующей итерации

### for()

### for(const item of obj)

### .forEach

итерирует, ничего не возвращает

### .map

вернет массив с изначальными элементами, изменными согласно callback'у

### .filter

вернет массив из начальных элементов, соответсвующих условию из callback'а

### .reduce

вернет аккумулированное(накопленное) значение, проходя массив слева-направо

### .reduceRight

Метод reduceRight() применяет функцию к аккумулятору и каждому значению массива (справа-налево), сводя его к одному значению.

</details>

<details>
<summary>Как сравнить на равенство строки\массивы\объекты?</summary>

-   == для чисел и строк
-   массивы поэлеметно
-   объекты рекурсивно (\_.isEqual из lodash)
-   == делает приведение типов (строка 5 === число 5)
-   === не делает приведение типов (строка 5 !== число 5)
</details>

<details>
<summary>Как можно объявить переменную? Их отличия.</summary>

```js
a = 5 // the same as var
var b = 10 // function scoped, hoisted, but undefined till initialization
let c = 20 // block scoped,
const d = 35 // block scoped constant, immutable
const obj = { age: 20 }
obj.age = 30 // no errors
```

</details>

<details>
<summary>null vs undefined?</summary>

null может ставить только сами\
undefined присваевается переменной после объявления, если конкретного присвоения не было

</details>

<details>
<summary>Отличие function declaration от function expression</summary>

declaration:

-   is hoisted

expression:

-   not hoisted = available after initialization

</details>

<details>
<summary>Отличие arrow функций от функци через function</summary>

arrow:

-   нельзя использовать arguments
-   синтаксис
-   нет своего this, this берется снаружи
-   не могут быть вызваны с помощью new

</details>

<details>
<summary>Что такое замыкание?</summary>

это возможность использовать в функции переменные, объявленные в родительских scop'ах

это функция вместе со всеми внешними переменными, которые ей доступны

</details>

<details>
<summary>Что такое шаблонные литералы? Для чего нужны</summary>

обратыне кавычки внутри которых пишут текст = ``

-   позволяют удобно конкатанировать переменные через конструкцию ${} вместо 'text' + 'other text'
-   позволяют делать переносы строк

</details>

<details>
<summary>Что такое Set, Map? для чего предназначены</summary>
Map - коллекция ключ \ значение как и Object\
Map однако позволяет использовать ключи любого типа, а не только string\

Set - коллекция, где каждое из значений может появляться только 1 раз

</details>

<details>
<summary>Что такое WeakSet, WeakMap? для чего предназначены</summary>
</details>

<details>
<summary>Как определить наличие поля в объекте?</summary>

```js
const obj = {
	a: 5,
	b: "text",
}

console.log(obj.hasOwnProperty("a")) // true only if object, not in proto
console.log("b" in obj) // true even if in protoype
```

</details>

<details>
<summary>Как создать объект?</summary>

С помощью литеральной нотации

```js
const myObj = {
	a: 5,
	b: "text",
}
```

С помощью функций

```js
function User(name, surname) {
	this.name = name
	this.surname = surname
}
const user = new User("Bob", "Dillinger")
console.log(user) // User {name: "Bob", surname: "Dillinger}
// class alike
```

С помощью класса

```js
class User {
	constructor(name, surname) {
		this.name = name
		this.surname = surname
	}
}
const user = new User("Bob", "Dillinger")
console.log(user) // User {name: "Bob", surname: "Dillinger}
// class instance
```

</details>

<details>
<summary>Какие значения falsy (ложные)?</summary>

-   null
-   undefined
-   0
-   ''
-   false
-   NaN
-   BigInt(0)

</details>

<details>

<summary>Зачем нужны операторы spread, rest?</summary>

spread - чтбы разворачивать массивы и объекты\
rest - принимает в себя все аргументы, невошедшие в параметры функции

</details>

<details>
<summary>Что такое деструктуризация?</summary>

синтаксис, который позволяет распаковать массив или объект в кучу переменных

```js
const arr = ["first", "second"]
const [first, second] = arr

const obj = { name: "first", age: "30" }
const { name, age } = obj
```

</details>

<details>
<summary>Какие объекты не serialazable в JS?</summary>
- function
- class instances
- map, weakmap
- set, weakset
- symbols
- promise
- ?bigint maybe?

</details>

<details>
<summary>Как копировать объект, избегая ссылочной зависимости? </summary>

```js
const shallowCopy = Object.assign({}, obj)
const shallowCopy2 = structuredClone(obj)

// only if obj is serializable
const deepCopy = JSON.parse(JSON.stringify(obj))

// lodash method
const deepCopy = _.deepClone(obj)
```

</details>

<details>
<summary>Как поменять контекст функции?</summary>

bind, call (rest), apply(array)

```js
function checkAge(a, b) {
	console.log(this) // { name: 'teenager', age: 15 }
	return a <= this.age && this.age <= b
}
const obj = { name: "teenager", age: 15 }
const objAgeChecker = checkAge.bind(obj, 15, 15)
console.log(objAgeChecker()) // true
```

call

```js
function checkAge(a, b) {
	console.log(this) // { name: 'teenager', age: 15 }
	return a <= this.age && this.age <= b
}
const obj = { name: "teenager", age: 15 }
console.log(checkAge.call(obj, 15, 15)) // true
```

apply

```js
function checkAge(a, b) {
	console.log(this) // { name: 'teenager', age: 15 }
	return a <= this.age && this.age <= b
}
const obj = { name: "teenager", age: 15 }
console.log(checkAge.apply(obj, [15, 15])) // true
```

</details>

<details>
<summary>Какие есть способы рабоыт с асинхронным кодом?</summary>

-   callback функция - передается в асинхронную функцию и вызывается внутрии неё, когда та посчитает нужным (скорее в конце своего выполнения)
-   Promise
-   async\await сахар над Promise
</details>

<details>
<summary>Что такое Promise</summary>

специальный объект, предназначенный для работы с асинхронным кодом\
принимает в себя в качесте аргумента функцию из аргументов в две callback функции = resolve, reject, которые следует вызвать для завершении работы функции и передачи значения

</details>

<details>
<summary>Зачем нужны Promise. Как использовать async\await.</summary>

Решают проблему глубокой вложенности (callback hell)\
Promise возвращает

```js
{
    value: ...,
    status: 'fulfilled' // или 'rejected' или 'pending'
}
```

async\await = синтаксический сахар для promise

async функция всегда вернет Promise

</details>

<details>
<summary>Зачем нудны Promise. all,allSettled, race</summary>

-   all ждет выполнения всех, но остановится и вернет ошибку если такая вознкнет
-   allSettled ждет выполнения всех, даже если возникла ошибка
-   race ждет выполнения первого промиса, возвращает его результат

```js
{
    value: ...,
    status: 'fulfilled' // или 'rejected' или 'pending'
}
```

</details>

<details>
<summary>для чего нужны e.preventDefault(), e.stopPropagation() </summary>

e.preventDefault - функция для предотвращения выполнения дефолтных реакций\действий бразуера по умолчанию на событие e
e.stopPropagation - функция для предотвращения всплытия (bubbling) события вверх по дереву ДОМ, т.е. чтобы родитель не узнал о событиях ребенка

</details>

<details>
<summary>Как отслеживать и обрабатывать ошибки в js?</summary>

с помощью блока try-catch

```jsx
try {
	// code where error may be thrown
} catch (err) {
	// code which will run if error is thrown
} finally {
	// code which will run in any case
}
```

</details>

<details>
<summary>Что такое прототипное наследование?</summary>
Все функции, классы, объекты, массивы = объекты на самом деле

Они наследуют поля от базовых объектов

</details>
