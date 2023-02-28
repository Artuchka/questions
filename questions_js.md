# Interview Questions about JS

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
<summary>Какие есть способы обхода массивов?</summary>

-   while{} do()
-   do() while{}
-   for()
-   for(of)
-   .forEach - итерирует, не возвращает
-   .map - вернет массив с элементами, изменными согласно callback'у
-   .filter - вернет массив, не соответсвующие условию из callback'а
-   .reduce - вернет аккумулированное(накопленное) значение
-   .reduceRight

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
<summary>Promise async\await.</summary>

Решают проблему глубокой вложенности (callback hell)
Promise возвращает

```js
{
    value: ...,
    status: 'fulfilled' // или 'rejected' или 'pending'
}
```

async\await = синтаксический сахар для promise

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
