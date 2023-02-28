# Interview Questions for JS, React Developer

<details>
  <summary>Что вызывает обновления компонентов?</summary>

Рендеринг не обязательно === отрисовка

(for ex: в список тасок добавилась 1 новая таска)

произойдёт рендеринг всех тасок, но отрисуется только новая

-   Обновление родителя
-   Обновление пропсов от родителя
-   Изменение стейта через setState
-   forceUpdate в классовых либо useReducer trick в функциональных

```js
const [, forceUpdate] = useReducer((x) => x + 1, 0)
```

может пригодится если обновления от setState не приходят, т.е. если ссылка на стейт остается той же

for ex:

```js
setState((prev) => Object.assign(prev, { key: "value" }))
forceUpdate()
```

</details>

<details>
  <summary>Что такое Virtual DOM? Как он работает?</summary>

Это вирутальное представление реального DOM дерева внутри реакта для повышения производительности при манипуляциях с DOM, т.к. делать каждый раз querySelector и т.д. довольно затратная операция.
Оно хранится в памяти.

Синхронизация с реальным DOM происходит засчёт Reconcilation

Fiber хранит доп. информацию о дереве - часть виртуального дома

Fiber это js объект, которая содержит инфу о компоненте, входные параметры(пропсы) и результат

todo: узнать как fibers связаны с обновлениями

</details>

<details>
  <summary>setState асинхронный или синхронный?</summary>
Асинхронный.

Однако react может объединять пачку вызовов setState в один рендер, не приводя к большому кол-ву ререндеров\
Это называется batching\

</details>

<details>
  <summary>Что такое JSX?</summary>

Javascript XML\
Дополнение к синтаксисиу JS\
Синтаксический сахар для функции React.createElement(component, props, children)\

JSX дает декларативность\
За транспайлинг (перевод из jsx в createElement) отвечает Babel\

```javascript
<div>Hello world</div>
```

будет преобразован в

```javascript
React.createElement("div", null, "Hello world")
```

</details>

<details>
  <summary>Разница memo и useMemo?</summary>

-   memo - HOC для мемоизации вложенного компонента при неизменных входных параметрах (пропсах), помогает избегать повторного рендеринга\
    сравнение пропсов поверхностное - shallow\

-   useMemo - hook для мемоизации значения функции, которая считается сильноэнергозатратной

```javascript
export default React.memo(Component, (prevProps, nextProps) => {
	// return true or false = wheter to update
})
```

```javascript
const result = useMemo(() => complexCalculations(id), [id])
```

</details>

<details>
  <summary>Что такое Pure Component?</summary>

Pure Component = "Чистый компонент"\
Называется таковым, если возвращает один и тот же результат при одинаковых пропсов и стейтов\

-   есть shouldComponentUpdate в классовых компонентах
-   есть React.memo в функциональных компонентах

```javascript
export default React.memo(Component, (prevProps, nextProps) => {
	// return true or false = wheter to update
})
```

```javascript
const result = useMemo(() => complexCalculations(id), [id])
```

</details>

<details>
  <summary>Отличия Redux и MobX?</summary>

MobX:

-   Несколько сторов
-   observables
-   неявноме изменение состояний - через подписки
-   меньше пустого boilerplate кода

Redux:

-   джс объекты для хранения
-   отслеживание измененй явное
-   Один стор
-   чистые функции для изменения состояний

</details>

<details>
  <summary>contextAPI + useReducer?</summary>

не позволяют time trabel debug\
нет dev tools\
подойдут для graphQL ApolloClient\

</details>

<details>
  <summary>Что такое HOC?</summary>

higher order component - компонент высшего порядка - компонент "обёртка"

это функция, получающая функцию и возвращающая её же, но модифицированную

-   для повторного использования ЛОГИКИ
-   для инжектирования зависимостей
-   помогает соблюдать DRY концепт

observer из MobX\
memo из React\
mapStateToProps из Redux для классовых компонентов\

hoc это фабрика компонентов\
hoc позволяет скрывать источники данных для компонентов\

```javascript
const _log = (type, msg) => console[type](msg)

const withLoggerHOC = (Comp) => {
	return (...props) => <Comp log={_log} {...props} />
}

export const ComponentForExport = withLoggerHOC(({ log, text }) => {
	useEffect(() => {
		log && log("error", "Some error " + text)
	}, [])

	return <h1>{text}</h1>
})
```

</details>

<details>
  <summary>Когда использовать классовые компонентов вместо функциональных?</summary>

В целом, почти все методы жизненного цикла (из классовых) можно так или иначе реализовать в функциональных

useEffect = componentWillUpdate, componentDidUpdate, componentDidMount

useLayoutEffect = componentDidUpdate, componentDidMount

однако отсутсвует несколько этапов жизнненного цикла из классовых в функ-ых

for ex: getSnapshotBeforeUpdate, getDerrivedStateFromError, componentDidCatch

    getSnapshotBeforeUpdate:

-   прямо перед добавлением в DOM
-   доступны некая инфа о ДОМ - положение прокрутки for ex

    getDerrivedStateFromError:

-   после возникновения ошибки потомка
-   вызывается на этапе рендера
-   для рендеринга запасного UI в случае ошибки

    componentDidCatch:

-   после возникновения ошибки потомка
-   для логгирования ошибка

errorBoundaries (предохранители) можно сделать только в классовых

как насчет

```javascript
<Suspense />
```

однако....?

</details>
<details>
  <summary>Зачем lazyLoading и codeSplitting?</summary>

lazyLoading:

-   для загрузки бандла (результата от webpack'a) только по требованию
-   для оптимизации приложения

Реализация

```javascript
const MaybeNotNeccesaryComponnet = React.lazy(() =>
	import("./components/MaybeNotNeccesaryComponnet")
)

const Comp = () => {
	return (
		<Suspense fallback={<SomeComponentWhileLazyCompIsLoading />}>
			<MaybeNotNeccesaryComponnet />
		</Suspense>
	)
}
```

</details>
