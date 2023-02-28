# Interview Questions about CSS

<details>
<summary>Что такое адаптивная верстка</summary>

создание разных layout'ов сайта в зависимости от размеров экрана

отдельная верстка под каждый viewport

</details>

<details>
<summary>Что grid, flex?</summary>

grid - двумерный layout
flex - одномерный layout либо столбец, либо строка

</details>

<details>
<summary>
    Что box model
</summary>

Модель, описывающая размеры элементы
margin, padding, width, height, border

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

margin - внешние отступы
padding - внутренний отступы

margin collapse - случай, когда внешние отступы соседних блоков не складываются, а выбирается наибольший из них
у padding такого нет

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
Как считается специфичность?
</summary>

-   нижние декларации важне верних

У каждого селектора свой вес:

-   \* = 0
-   div = h1 = p = 1
-   .message = :hover = :active = 10
-   #message = 100
-   inline styles = 1000
-   important = 1000

inline стили это

```html
<div style="display: flex;">text</div>
```

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

псевдоэлементов только 2:

-   before
-   after

</details>

<details>
<summary>Какие значения есть у display? Как они работают</summary>

-   block = блочный элемент стремится занять всё доступное пространство, блочные элементы располагаются один над другим вертикально
-   inline = элемент занимает пространство по своему содержимому, остается в строке с другими inline элементами, нельзя менять ширину и высоту
-   inline-block = элемент занимает пространство по своему содержимому, остается в строке с другими inline элементами, можно менять ширину высоту
-   flex = располагает детей элемента в строку или столбец и позволяет манипулировать их положением
-   grid = располагает детей элемента по двумерной сетке и позволяет манипулировать их положением
-   none = не отображает элемент, убирает его из ДОМ дерева
-   initial = то значение display, которое присуще этому тегу по умолчанию
</details>

<summary>Какие значения есть у position? Как они работают</summary>

-   static = значение по умолчанию, распологается последовательно в ДОМ дереве
-   relative = оставляет элемент в потоке ДОМ дерева, позволяет изменять положение относительно исходного
-   absolute = вырывает элемент из потока ДОМ дерева, позволяет изменять положение в пределах документа или ближайшего позиционированного родителя (со значением position !== static)
-   fixed = вырывает элемент из потока ДОМ дерева, позволяет изменять положение в пределах экрана или ближайшего позиционированного родителя со значением (position !== static)
-   sticky = оставляет элемент в потоке ДОМ дерева, позволяет изменять положение в рамках родителя на определенном расстоянии от краёв этого родителя
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
