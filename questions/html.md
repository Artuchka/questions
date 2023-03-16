# Interview Questions about HTML

<details>
<summary>
Что такое DOCTYPE? Для чего он используется?
</summary>

Это тег, указывающий тип документа, на версию HTML\
добавляется первой строкой HTML \ XML документа

Нужен для бразуера при парсинге кода страницы
Определяет устаревшие \ валидные теги

</details>

<details>
<summary>
Что такое DOM дерево?
</summary>

Дерево, показывающее зависимости между блоками (вложенность)

Объектная модель документа, создаваемая браузером при парсинге

</details>

<details>
<summary>
Опишите базовую структура HTML документа.
</summary>

```html
<!-- версия документа -->
<!DOCTYPE html>

<html lang="en">
	<!-- не отрисовывется на странице -->
	<head>
		<!-- SEO информация для роботов поисковиков -->
		<!-- загрузка шрифтов -->
		<!-- загрузка скриптов -->
		<!-- подгрузка других файлов -->
		<meta charset="UTF-8" />
		<title>Document Tab title</title>
	</head>

	<!-- отрисовывется на странице -->
	<body>
		<h1>Hello world</h1>
	</body>
</html>
```

</details>

<details>
<summary>
Что такое семантика?
Семантическая верстка. Зачем нужна?
Какие семантические теги знаете?
</summary>

Семантика - использование правильных тегов, описывающих содержимый контент внутри себя
Семантический тег - тег, носящий смысловое осмысление, имеющие предопределенные css-свойства.

-   позволяет screen reader'ам понимать правильную структуру сайта
-   => accesability (доступность) повышается
-   позволяет улучшить SEO для роботов google, bing, yandex...
-   для ориентации в разметке
-   => позволяет понять что находится внутри тега, без лишних комментариев

```html
<!-- не просто <div></div>, а: -->
<header></header>
<footer></footer>
<section></section>
<article></article>
<aside></aside>
<main></main>
<nav></nav>

<p></p>
<h1></h1>
<title></title>
<a></a>
<span></span>
<strong></strong>
<small></small>
```

</details>

<details>
<summary>
Разница между `<strong>` `<em>` и `<b>` `<i>`
</summary>

strong и em дают логическое выделение\
дают семантику, а не просто визуально выделяют

значит поисковые роботы сделают акцент на этих словах

</details>

<details>
<summary>
Что такое валидация? Какие типы проверов в HTML документах?
</summary>

Проверка на стандарты от W3C

Определение типа документа

Проверка:

-   отуствие html тегов
-   вложенности
-   синтаксиса
-   DTD = document type definition
-   посторонние элементы

</details>

<details>
<summary>
Какой тег для кнопки?
</summary>

```html
<!-- 1 -->
<button>Click me</button>

<!-- 2. in forms -->
<button type="submit">Click me</button>
<input type="submit" value="Click me" />

<!-- 3. input button -->
<input type="submit" />

<!-- 4. link button  -->
<a href="#">Click me</a>
```

</details>

<details>
<summary>
Как семантически правильно сверстать картинку с подписью?
</summary>

```html
<figure>
	<img src="src-for-size_1" alt="Image of size 1" />
	<img src="src-for-size_2" alt="Image of size 2" />

	<figcaption>
		<h1>Да, я заголовок подписи картинки</h1>
		<p>А я разъясняющий текст подписи картинки</p>
	</figcaption>
</figure>
```

</details>

<details>
<summary>
Разница `sciprt`, `script async`, `script defer`
</summary>

-   script блокирует дальнейшее чтение документа до момента своего полного исполнения (такие лучше добавлять в конце перед `</body>`)

-   script async запускает отдельный поток для загрузки скрипт-файла, который выполнится после своей загрузки в любой момент (подходит для независимых скриптов = аналитика)

-   script defer запускает отдельный поток для загрузки скрипт-файла, который выполнится после загрузки и после того, как DOM-дерево готово (подходит для скриптов, работающих с DOM-элементами)

</details>

<details>
<summary>
Зачем нужен datalist?
</summary>

Для создания выпадающего списка на обычном input'е, предлагающего автозаполнение из представленных вариантов,
перечисленных в datalist

</details>
