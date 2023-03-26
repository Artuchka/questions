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
bigintNumber = 1322n // для больших чисел произвольной длины
obj = { name: "temka" }
symb = new Symbol("some string; я возвращаю уникальную ссылку")
```

</details>

<details>
<summary>
Что такое область видимости (Scope)?
</summary>

Место в программе, синтаксически ограниченное скобочками `{ ...some code... }` И ОБЯЗАТЕЛЬНО имеющий объявленные переменные внутри себя

Если без объявлений, то это просто block

Виды scope'ов (видимостей):

-   Global scoped
-   Function scoped
-   Block scoped

</details>

<details>
<summary>Что такое this?</summary>

ключевое слово котороые ссылается на контекст вызова\
который выполняет или вызывает функцию\
в глобальной обласи видимости

-   в браузере this = window
-   в nodejs this = globalThis

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

В пределах функции значение this зависит от того, каким образом вызвана функция:

-   Простой вызов - В этом случае значение this не устанавливается вызовом. Так как этот код написан не в строгом режиме, значением this всегда должен быть объект, по умолчанию - глобальный объект. В строгом режиме, значение this остается тем значением, которое было установлено в контексте исполнения. Если такое значение не определено, оно остается undefined. Для того что бы передать значение this от одного контекста другому необходимо использовать call или apply

-   В стрелочных функциях, this привязан к окружению, в котором была создана функция. В глобальной области видимости, this будет указывать на глобальный объект.

-   Когда функция вызывается как метод объекта, используемое в этой функции ключевое слово this принимает значение объекта, по отношению к которому вызван метод.

</details>

<details>
<summary>Как поменять контекст функции?</summary>

bind, call (rest), apply(array)

bind\
bind не вызывает функцию. Он только возвращает «обёртку», которую мы можем вызвать позже, и которая передаст вызов в исходную функцию, с привязанным контекстом.

```js
function checkAge(a, b) {
	console.log(this) // { name: 'teenager', age: 15 }
	return a <= this.age && this.age <= b
}
const obj = { name: "teenager", age: 15 }
const objAgeChecker = checkAge.bind(obj, 15, 15)
console.log(objAgeChecker()) // true
```

call\
call сразу вызывает функцию, первый аргумент call становится её this, а остальные передаются «как есть». Вызов func.call(context, a, b...) – то же, что обычный вызов func(a, b...), но с явно указанным this(=context)

```js
function checkAge(a, b) {
	console.log(this) // { name: 'teenager', age: 15 }
	return a <= this.age && this.age <= b
}
const obj = { name: "teenager", age: 15 }
console.log(checkAge.call(obj, 15, 15)) // true
```

apply\
отличается от call тем, что принимает аргументы в виде массива

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
<summary>Что такое строгий режим (strict mode)?</summary>

дает ошибки там, где обычный js engine не выдает ошибки (а стоило бы)

3 варианта включения такого режима:

1.  at the top of file
    ```js
    "use-strict"
    ```
2.  inside function
    ```js
    function func() {
    	"use-strict"
    }
    ```
3.  by default inside ESM

    ```js
    function func() {}

    export default func
    ```

</details>

<details>
<summary>Какие есть способы обхода массивов? Их особенности</summary>

##### while() do{}

сначала проверятся условие, потом делается итерация

##### do() while{}

первая итерация будет в любом случае, проверятся условие для следующей итерации

##### for(startAction; condition; actionForEveryLoop)

обычный классический цикл for

##### for(const key in obj) - лучше для объектов

##### for(const item of array) - лучше для массивов

```js
Object.prototype.objCustom = function () {}
Array.prototype.arrCustom = function () {}

let iterable = [3, 5, 7]
iterable.foo = "hello"

for (let i in iterable) {
	console.log(i) // выведет 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
	if (iterable.hasOwnProperty(i)) {
		console.log(i) // выведет 0, 1, 2, "foo"
	}
}

for (let i of iterable) {
	console.log(i) // выведет 3, 5, 7
}
```

##### .forEach

итерирует, ничего не возвращает, нельзя остановить через `break`, `return`, `continue`, неаккуратно каждый второй брать i+=2

##### .map

вернет массив с изначальными элементами, изменными согласно callback'у

##### .filter

вернет массив из начальных элементов, соответсвующих условию из callback'а

##### .reduce

вернет аккумулированное(накопленное) значение, проходя массив слева-направо

##### .reduceRight

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

null:

-   можем ставить только сами
-   явное задание отсутсвующие значение

undefined:

-   присваевается переменной после объявления, если конкретного присвоения не было
-   функция которая ничего не возращает явно
-   несуществующее свойство функции
</details>

<details>
<summary>Что такое hoisting (поднятие)?</summary>

Hoisiting = Процесс, происходящий во 2-ой (из 3-ёх) фазе компиляции (Parsing + creating AST)\
авто-регистрация имён и присвоение им значений, как только программа "входит" в scope

`var` хоистится со значением undefined вплоть до своего declaration
`function` declaration хоистится со значением ссылки на функцию в начале scope'а
`let` и `const` хоистится = авто-регистрируется, но не авто-инициализируется до своего declaration

</detials>

<details>
<summary>Отличие function declaration от function expression</summary>

declaration:

-   is hoisted (auto-registered + auto-inited function reference)

expression:

-   not hoisted = available only after initialization (auto-registered but not auto-inited till declaration line)

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

это комбинация функции и лексического окружения, в котором эта функция была объявлена. Это окружение состоит из произвольного количества локальных переменных, которые были в области действия функции во время создания замыкания.

</details>

<details>
<summary>Что такое шаблонные литералы? Для чего нужны</summary>

обратыне кавычки внутри которых пишут текст = ``

-   позволяют удобно конкатанировать переменные через конструкцию ${} вместо 'text' + 'other text'
-   позволяют делать переносы строк

</details>

<details>
<summary>Что такое Set, Map? для чего предназначены</summary>
Map - коллекция вида ключ:значение как и Object\
Map однако позволяет использовать ключи любого типа, а не только string\

```js
let map = new Map()

map.set("1", "str1") // ключ-строка
map.set(1, "num1") // число
map.set(true, "bool1") // булевое значение

// в обычном объекте это было бы одно и то же,
// map сохраняет тип ключа
alert(map.get(1)) // 'num1'
alert(map.get("1")) // 'str1'

alert(map.size) // 3
```

Set - коллекция, где каждое из значений может появляться только 1 раз

```js
let set = new Set()

let vasya = { name: "Вася" }
let petya = { name: "Петя" }
let dasha = { name: "Даша" }

// посещения, некоторые пользователи заходят много раз
set.add(vasya)
set.add(petya)
set.add(dasha)
set.add(vasya)
set.add(petya)

// set сохраняет только уникальные значения
alert(set.size) // 3

set.forEach((user) => alert(user.name)) // Вася, Петя, Даша
```

</details>

<details>
<summary>Что такое WeakSet, WeakMap? для чего предназначены</summary>

WeakSet – особый вид Set, не препятствующий сборщику мусора удалять свои элементы. То же самое – WeakMap для Map. То есть, если некий объект присутствует только в WeakSet/WeakMap – он удаляется из памяти. Это нужно для тех ситуаций, когда основное место для хранения и использования объектов находится где-то в другом месте кода, а здесь мы хотим хранить для них «вспомогательные» данные, существующие лишь пока жив объект. Например, у нас есть элементы на странице или, к примеру, пользователи, и мы хотим хранить для них вспомогательную информацию, например обработчики событий или просто данные, но действительные лишь пока объект, к которому они относятся, существует. Если поместить такие данные в WeakMap, а объект сделать ключом, то они будут автоматически удалены из памяти, когда удалится элемент. Например:

```js
let activeUsers = [{ name: "Вася" }, { name: "Петя" }, { name: "Маша" }]

// вспомогательная информация о них,
// которая напрямую не входит в объект юзера,
// и потому хранится отдельно
let weakMap = new WeakMap()

weakMap.set(activeUsers[0], 1)
weakMap.set(activeUsers[1], 2)
weakMap.set(activeUsers[2], 3)
weakMap.set("Katya", 4) //Будет ошибка TypeError: "Katya" is not a non-null object

alert(weakMap.get(activeUsers[0])) // 1

activeUsers.splice(0, 1) // Вася более не активный пользователь

// weakMap теперь содержит только 2 элемента

activeUsers.splice(0, 1) // Петя более не активный пользователь

// weakMap теперь содержит только 1 элемент
```

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

!!!!Truthy!!!!:

-   {}

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
<summary>Какие есть способы рабоыт с асинхронным кодом?</summary>

-   callback функция - передается в асинхронную функцию и вызывается внутрии неё, когда та посчитает нужным (скорее в конце своего выполнения)
-   Promise
-   async\await сахар над Promise
</details>

<details>
<summary>Что такое Promise</summary>

специальный объект, предназначенный для работы с асинхронным кодом\
принимает в себя в качесте аргумента функцию из аргументов в две callback функции = resolve, reject, которые следует вызвать для завершении работы функции и передачи значения

Promise возвращает

```js
{
    value: ...,
    status: 'fulfilled' // или 'rejected' или 'pending'
}
```

Вначале status = pending («ожидание»), затем – одно из: fulfilled («выполнено успешно») или rejected («выполнено с ошибкой»).

```js
// Создаётся объект promise
let promise = new Promise((resolve, reject) => {
	setTimeout(() => {
		// переведёт промис в состояние fulfilled с результатом "result"
		resolve("result")
	}, 1000)
})

// promise.then навешивает обработчики на успешный результат или ошибку
promise.then(
	(result) => {
		// первая функция-обработчик - запустится при вызове resolve
		alert("Fulfilled: " + result) // result - аргумент resolve
	},
	(error) => {
		// вторая функция - запустится при вызове reject
		alert("Rejected: " + error) // error - аргумент reject
	}
)
```

</details>

<details>
<summary>Зачем нужны Promise. Как использовать async\await.</summary>

Решают проблему глубокой вложенности (callback hell)\

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
<summary>Что такое EventPropagation?</summary>

"Распостранение События" по-русски

Событие, совершенное в ребенке, заставляет совершится такое же событие в родителе

Происходит в 3 фазы:

1. Capture Phase = Фаза захвата (от window до элемента)
2. Target Phase (внутри целевого элемента)
3. Bubbling Phase = Фаза всплытия (от элемента до window)

1 шаг затрагивает всех предков целевого элемента

</details>
<details>
<summary>Что такое Event Delegation?</summary>

"Делегирование События" по-русски

Приём в разработке, позволяющий оптимизровать исполнение событий

Вместо добавления однотипного события к каждому (например, из сотен) детей\
Событие добавляется только к родителю

Отслеживание задетого ребёнка делают через e.target

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
<summary>Что такое Higher Order Functions?</summary>

-   Функция которая возращает другую функцию
-   Функция которая принимает другую функцию в качестве аргумента

for ex:

-   map
-   filter
-   forEach
-   reduce

</details>

<details>
<summary>Что такое чистая Pure функция?</summary>

-   Не имеет побочных эффектов
    -   Изменение в файловой системе
    -   HTTP запросы
    -   Изменение параметров
    -   Вывод на экран
-   Всегда возращает одинаковый результат при одинаковых аргументах (независимость от внешних состояний)

</details>

<details>
<summary>Что такое прототипное наследование?</summary>
Все функции, классы, объекты, массивы = объекты на самом деле

Они наследуют поля от базовых объектов

</details>

<details>
<summary>Что такое статический метод класса (static)?</summary>
Ключевое слово static используется в классах для определения статичных методов. Статичные методы функции, принадлежащие объекту класса, но не доступные другим объектам того же класса.

```js
class Repo {
	static getName() {
		return "Repo name is modern-js-cheatsheet"
	}
}

// нам не нужно создавать объект класса Repo
console.log(Repo.getName()) // "Repo name is modern-js-cheatsheet"

let r = new Repo()
console.log(r.getName()) // необработанная ошибка TypeError: r.getName не является функцией
```

Cтатические методы вызываются через имя класса. Вызывать статические методы через имя объекта запрещено. Статические методы часто используются для создания вспомогательных функций приложения.

</details>
