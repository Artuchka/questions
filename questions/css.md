# Interview Questions about CSS

<details>
<summary>
Что такое CSS? Для чего используется?
</summary>

Cascading Style Sheets - каскадные таблицы стилей

Список правил, определяющих стили для DOM элементов страницы
Добавляет визульные эффекты для страницы

</details>

<details>
<summary>
Варианты добавления CSS стилей на страницу?
</summary>

-   внутри тега `style` в `head` теге (для всей страницы = global)
-   внутри `html template` тоже через тег `style` (только для этого `template` элемента)
-   через тег `link` в `head` теге (external file)
-   inline-стили через аттрибут `style` у любого html DOM элемента
-   импорт css файла внутрь главного css файла, который уже добавлен в документ
-   импорт css файла внутрь js скрипта, который включается сборщиком(bundler'ом) в бандл

inline-стили можно переопредлить только через !important

</details>

<details>
<summary>Разница адаптивного и респонсив дизайна</summary>

Adaptive:

-   создание разных layout'ов сайта в зависимости от размеров экрана
-   отдельная верстка под каждый viewport

Responsive:

-   перестройка верстки под каждый viewport

</details>

<details>
<summary>Что grid, flex?</summary>

grid - двумерный layout
flex - одномерный layout либо столбец, либо строка

</details>

<details>
<summary>
    Что box model (блочная модель) CSS?
</summary>

Модель, описывающая размеры элемента\
margin, padding, width, height, border\
Рассчитывает сколько пространства будет занимать элемент

```css
box-sizing: border-box;
```

в width и height включает padding и border

```css
box-sizing: content-box;
```

в width и height НЕ включает padding и border

</details>

<details>
<summary>Разница margin, padding?</summary>

-   margin - внешние отступы
-   padding - внутренний отступы

-   margin collapse - случай, когда внешние отступы соседних блоков не складываются, а выбирается наибольший из них
-   у padding такого нет

</details>

<details>
<summary>Разница display:none, visibility: hidden?</summary>

оба скрывают элемент для юзера

display: none:

-   удаляет из нормального потока документа
-   соседи занимают его место
-   доступен в DOM дереве
-   скрыт для поисковых роботов

visibility: hidden:

-   оставляет в нормальном потоке документа
-   занимает своё место
-   открыт для поисковых роботов

</details>

<details>
<summary>Из чего строится размер элемента?</summary>

-   content
-   paddng inline + padding block
-   border inline + border block
-   margin inline + margin block

</details>

<details>
<summary>За что отвечает z-index?</summary>

управляет расположением позиционированных блоков(не static), поперечно экрану
больший z-index => ближе к пользователю
меньший z-index => дальше от пользователя

</details>

<details>
<summary>
Что такое селектор? Какие селекторы существуют?
</summary>

Селектор - часть CSS правила, определяющая цель применения правила

Простые:

```css
.class {
}
#id {
}
p {
}
* {
}
a[href="test"] {
}
```

Составные:

```css
h1,
h3,
span {
}
div p {
}
li > a {
}
a:hover {
}
li:nth-last-child(2n) {
}
```

</detials>

<details>
<summary>
Что такое вендорный префикс?
</summary>

Приставка к css свойству, поддерживающее значение свойства для браузеров, где это свойство не внедрено на постоянной основе (testing or developing)

```css
.class {
	-webkit-opacity: 0.5; /* Chrome + Safari */
	-moz-property: 0.5; /* Firefox */
	-ms-property: 0.5; /* Internet Explorer, Edge */
	-o-property: 0.5; /* Opera */
	property: 0.5;
}
```

</detials>

<details>
<summary>
Разница Progressive Enhancement и Graceful Degradation?
</summary>

Progressive Enhancement = "прогрессивное улучшение" aka mobile-first
Graceful Degradation = "плавная деградация" aka desktop-first

кстати, @supports + vendor prefix это Graceful Degradation

</detials>

<details>
<summary>
Как считается специфичность?
</summary>

-   нижние декларации важне верних

У каждого селектора свой вес:

-   \* => 0
-   div = h1 = p = ::before = ::after = [attr="smth"] => 1
-   .message = :hover = :active => 10
-   #message => 100
-   inline styles => 1000
-   important => 10000

inline стили это

```html
<div style="display: flex;">text</div>
```

как считать:
`
h1 + \*[href="#test-link"] span#coolSpan::after

1 (h1) + 0 (\*) + 1 ([attr]) + 1 (span) + 100 (#coolSpan) + 1 (::after)

=> 104
`

</details>

<details>
<summary>
Разница Reset.css и Normalize.css?
</summary>

Reset обнуляет все дефолтные стили для всех бразуеров

Normalize усредняет дефолтные стили для всех бразузеров

Reset - придется устанавливать заново
Normalize - популярнее

</details>

<details>
<summary>
Псевдоклассы и псевдоэлементы.
</summary>

псевдоклассы:

-   hover
-   active
-   focus
-   checked
-   disabled
-   nth-child
-   root
-   not()
-   is()
-   placeholder-shown
-   и др

псевдоэлементов только 5:

-   before
-   after
-   first-letter
-   first-line
-   selection

</details>

<details>
<summary>Какие значения есть у display? Как они работают</summary>

-   block = блочный элемент стремится занять всё доступное пространство, блочные элементы располагаются один над другим вертикально, можно менять ширину высоту
-   inline = элемент занимает пространство по своему содержимому, остается в строке с другими inline элементами, нельзя менять ширину и высоту, не работает margin-top margin-bottom
-   inline-block = элемент занимает пространство по своему содержимому, остается в строке с другими inline элементами, можно менять ширину высоту
-   flex = располагает детей элемента в строку или столбец и позволяет манипулировать их положением
-   grid = располагает детей элемента по двумерной сетке и позволяет манипулировать их положением
-   none = не отображает элемент, убирает его из ДОМ дерева
-   initial = то значение display, которое присуще этому тегу по умолчанию
</details>

<details>
<summary>Какие значения есть у position? Как они работают</summary>

-   static = значение по умолчанию, распологается последовательно в ДОМ дереве\
    top left right bottom z-index не применимы
-   relative = оставляет элемент в потоке ДОМ дерева, позволяет изменять положение относительно исходного\
    не заставляет двигаться соседние элементы при сдвиге
-   absolute = вырывает элемент из нормального потока ДОМ дерева(создает свой поток), позволяет изменять положение в пределах документа или ближайшего позиционированного родителя (со значением position !== static)
-   fixed = вырывает элемент из потока ДОМ дерева(создает свой поток), позволяет изменять положение в пределах экрана (window)
-   sticky = оставляет элемент в потоке ДОМ дерева, позволяет изменять положение в рамках родителя на определенном расстоянии от краёв этого родителя
    не заставляет двигаться соседние элементы при сдвиге

</details>

<details>
<summary>Что такое БЭМ?</summary>
Блок Элемент Модификатор

методология
подразумевает компонентный подход к верстке
помогает разворачивать сложные верстки

</details>

<details>
<summary>quest</summary>
</details>
