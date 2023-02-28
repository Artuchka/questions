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

включает margin, padding в размеры элемента, убавляя его width, height

</details>

<details>
<summary>
Как считается специфичность?
</summary>

\- = 0
div = h1 = p = 1
.message = :hover = :active = 10
#message = 100
inline styles = 1000
important = 1000

-   нижние декларации важне
-   !important

```css
box-sizing: border-box;
```

включает margin, padding в размеры элемента, убавляя его width, height

</details>

<details>
<summary>
Псевдоклассов и псевдоэлементов.
</summary>

псевдоклассы:

-   hover
-   active
-   focus
-   placeholder-shown
-   и др

псевдоэлементов только 2:

-   before
-   after

</details>
