# Interview Questions about React

<details>
  <summary>Что такое React? Для чего он нужен?</summary>

библиотека для создания пользовательских интерфейсов\

нужен для:

-   стандартизация разработки
-   позволяет удобно изменять интерфейс (значение ДОМ элемента) в зависимости от состояния
-   позволяет использовать декларативный подход
-   увеличивает производительность приложений:
    -   дает 60fps
    -   querySelector долгий для обращения к каждому элементу

</details>

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

Это вирутальное представление оригинального реального DOM дерева внутри реакта для повышения производительности при манипуляциях с DOM, т.к. делать каждый раз querySelector и т.д. довольно затратная операция.\
Оно хранится в памяти.\

-   Предназначено, что сосредоточится на логике взаимодействия с данными, а не манипуляцией реального ДОМ дерева
-   Применили такой подход, т.к. работа с ДОМ элементами сложная и малоэфективная

Синхронизация с реальным DOM происходит засчёт Reconcilation

Fiber хранит доп. информацию о дереве - часть виртуального дома

Fiber это js объект, которая содержит инфу о компоненте, входные параметры(пропсы) и результат

todo: узнать как fibers связаны с обновлениями

</details>

<details>
  <summary>Что такое React Reconciliation (Cверка) и как он работает?</summary>
Reconciliation (Cверка) - это процесс, посредством которого React обновляет DOM. Когда состояние компонента изменяется, React должен рассчитать необходимость обновления DOM. Это делается путем создания виртуального DOM и сравнения его с текущим DOM. В этом контексте виртуальный DOM будет содержать новое состояние компонента.

При сравнении двух деревьев первым делом React сравнивает два корневых элемента. Поведение различается в зависимости от типов корневых элементов.

Всякий раз, когда корневые элементы имеют различные типы, React уничтожает старое дерево и строит новое с нуля.

При сравнении двух React DOM-элементов одного типа, React смотрит на атрибуты обоих, сохраняет лежащий в основе этих элементов DOM-узел и обновляет только изменённые атрибуты.

По умолчанию при рекурсивном обходе дочерних элементов DOM-узла React проходит по обоим спискам потомков одновременно и создаёт мутацию, когда находит отличие. Эта неэффективность может стать проблемой. Когда у дочерних элементов есть ключи, React использует их, чтобы сопоставить потомков исходного дерева с потомками последующего дерева.

</details>

<details>
  <summary>Расскажите про жизненный цикл компонента и его методы.</summary>
  1. Initialization
	реакт готовит установку компонента со значениями по умолчанию
  2. Mounting
	componentWillMount, componentDidMount, компонент готов для монтирования
  3. Updation
	- новые свойства
	- новое состояние
	shouldComponentUpdate, componentDidUpdate, componentWillUpdate
  4. Unmounting
	- размонтирование
	componentWillUnmount
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
Дополнение к синтаксисиу JS, который позволяет писать HTML в React компонентах\
Синтаксический сахар для функции React.createElement(component, props, children)\

JSX дает декларативность\
За транспайлинг (перевод из jsx в createElement) отвечает Babel\

```javascript
<div className="greeting">Hello world</div>
```

будет преобразован в

```javascript
React.createElement("div", { className: "greeting" }, "Hello world")
```

</details>

<details>
  <summary>Что такое Babel и для чего он используется?</summary>

Babel.JS – это транспайлер, переписывающий код на ES-2015 в код на предыдущем стандарте ES5.

Обычно Babel.JS работает на сервере в составе системы сборки JS-кода (например webpack или brunch) и автоматически переписывает весь код в ES5.

Настройка такой конвертации тривиальна, единственно – нужно поднять саму систему сборки, а добавить к ней Babel легко, плагины есть к любой из них.

Конфигурация Babel прописывается в файле babel.config.js, либо в .babelrc для настроек одного пакета, а также в package.json или .babelrc.js

Пример конфига в babel.config.js:

```js
module.exports = function (api) {
    api.cache(true);

    const presets = [ ... ];
    const plugins = [ ... ];

    return {
      presets,
      plugins
    };
  }
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
  <summary>Что такое Flux - архитектура? Какие сущности она имеет?</summary>
  Flux-архитектура — архитектурный подход или набор шаблонов программирования для построения пользовательского интерфейса веб-приложений, сочетающийся с реактивным программированием и построенный на однонаправленных потоках данных.

Основной отличительной особенностью Flux является односторонняя направленность передачи данных между компонентами Flux-архитектуры. Архитектура накладывает ограничения на поток данных, в частности, исключая возможность обновления состояния компонентов самими собой. Такой подход делает поток данных предсказуемым и позволяет легче проследить причины возможных ошибок в программном обеспечении.

В минимальном варианте Flux-архитектура может содержать три слоя, взаимодействующие по порядку:

-   Действия (англ. actions) — выражение событий (часто для действий используются просто имена — строки, содержащие некоторый «глагол»). Диспетчеры передают действия нижележащим компонентам (хранилищам) по одному. Новое действие не передаётся пока предыдущее полностью не обработано компонентами. Действия из-за работы источника действия, например, пользователя, поступают асинхронно, но их диспетчеризация явлется синхронным процессом. Кроме имени (англ. name), действия могут иметь полезную нагрузку (англ. payload), содержащую относящиеся к действию данные.

-   Диспетчер/Диспатчер (англ. dispatcher) предназначен для передачи действий хранилищам. В упрощённом варианте диспетчер может вообще не выделяться, как единственный на всё приложение. В диспетчере хранилища регистрируют свои функции обратного вызова (callback) и зависимости между хранилищами.

-   Хранилище (англ. store) является местом, где сосредоточено состояние (англ. state) приложения. Остальные компоненты, согласно Flux, не имеют значимого (с точки зрения архитектуры) состояния. Изменение состояния хранилища происходит строго на основе данных действия и старого состояния хранилища при помощи чистых функций.

-   Представление (англ. view) — компонент, обычно отвечающий за выдачу информации пользователю. Во Flux-архитектуре, которая может технически не касаться внутреннего устройства представлений вообще, это — конечная точка потоков данных. Для информационной архитектуры важно только, что данные попадают в систему (то есть, обратно в хранилища) только через действия.
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
  <summary>Отличия классовых компонентов и функциональных?</summary>

-   синтаксис:
    -   Новый класс, с наследованием от класса React.Component, с методом render где лежит JSX
        ```javascript
        class extends React.Component {
        	render() {
        		text
        	}
        }
        ```
    -   Просто функция с большой буквы, которая возвращает JSX
        ```javascript
        const Component = () => {
        	retrun(<h1>text</h1>)
        }
        ```
-   прямые методы жизненного цикла есть только в классовых
-   для управления жизненным циклом используются хуки

-   для изменения состониями испольуется setState()
-   для изменения состониями испольуются хуки (useState, useReducer)

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
  <summary>Для чего используется useEffect?</summary>
  хук для синхронизации, как завещает Дэн Абрамов.

хук для выполнения сайд эффектов.

```jsx
useEffect(func, deps)
```

выполняет функцию func при первоначальном рендере компонента и изменении переменных из массива deps

```jsx
useEffect(() => {
	console.log("deps have chaned")
}, [])

useEffect(() => {
	console.log("new value = ", value)
}, [value])

useEffect(() => {
	console.log("new value = ", value)

	return () => {
		console.log("i will log when component unmounts")
	}
}, [value])
```

</details>

<details>
  <summary>Что такое side effects?</summary>

Действия (подписки, запросы данных, логирование и тп), которые мы хотим соверишть при измении данных (?, которые не связаны с сохранением данных?)

```jsx
useEffect(() => {
	// will run when comonent mounts and value1, value2 updated
}, [value1, value2])
```

</details>

<details>
  <summary>Как избежать утечек памяти с useEffect?</summary>

использовать clearFunction в useEffect, чтобы:

-   отменить подписки на некие изменения
-   убрать интервалы, таймауты
-   убрать eventListener'ы,

```jsx
useEffect(() => {
	console.log("new value = ", value)
	const interval = setInterval()
	const timeout = setTimeout()
	window.addEventListener()
	// preloading; initialization of data; etc...

	return () => {
		console.log("i will run when component unmounts")
		unsibscribe()
		clearInterval(interval)
		clearTimeout(timeout)
		window.removeEventListener()
	}
}, [])
```

</details>

<details>
  <summary>Какие паттерны есть в React?</summary>

-   controlled and uncontrolled components
-   higher order components
-   render props
-   compound components
</details>

<details>
  <summary>Примеры использования HOC в React?</summary>

</details>

<details>
  <summary>Примеры использования Render Props в React?</summary>

</details>

<details>
  <summary>Примеры использования Compound Components в React?</summary>

</details>

<details>
  <summary>Как передать данные между соседними компонентами?</summary>

-   через пропсы от единого родителя
-   через context
-   c помощью state manager'а
-   используя ref'ы в классовых компонентах

```jsx
class Parent extends React.PureComponent {
	titleRef
	saveTitleRef = (ref) => (this.titleRef = ref)
	getTitleRef = () => this.titleRef

	render() {
		return (
			<div>
				<Child refGetter={this.getTitleRef} />
				<div ref={this.saveTitleRef}>I am title</div>
			</div>
		)
	}
}

class Child extends React.PureComponent<any> {
	setWhatever = () => {
		this.props.refGetter().innerHTML = Math.random()
	}

	render() {
		return (
			<div>
				<button onClick={this.setWhatever}>Press me</button>
			</div>
		)
	}
}
```

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

<details>
  <summary>Что такое управляемые и неуправляемы компоненты?</summary>

управляемый - тот компонент, значение которого контроллирует React\
обычно для input textarea select

```javascript
const CompWithControlledInput = () => {
	const [name, setName] = useState("")

	const handleChange = (e) => {
		setName(e.target.value)
	}
	const handleSubmit = (e) => {
		e.preventDefault()
		console.log({ name })
	}
	return (
		<form onSubmit={handleSubmit}>
			<input value={name} onChange={handleChange} />
		</form>
	)
}

const CompWithUncontrolledInput = () => {
	const ref = useRef(null)

	const handleSubmit = (e) => {
		e.preventDefault()
		const name = ref.current.value
		console.log({ name })
	}
	return (
		<form onSubmit={handleSubmit}>
			<input ref={ref} value={name} onChange={handleChange} />
		</form>
	)
}
```

</details>

<details>
  <summary>Что такое context? Зачем он нужен?</summary>

context позволяет передавать данные по дереву компонентов без prop drilling'а
(без необходмости передачи пропсов на промежуточных компонентах) от родителя к дитю

```javascript
const ContextExampleApp = () => {
	// const [name, setName] = useState("")
	// const handleChange = (e) => {
	// 	setName(e.target.value)
	// }
	// const handleSubmit = (e) => {
	// 	e.preventDefault()
	// 	console.log({ name })
	// }
	// return (
	// 	<form onSubmit={handleSubmit}>
	// 		<input value={name} onChange={handleChange} />
	// 	</form>
	// )
}
```

</details>

<details>
  <summary>Что такое порталы? Для чего они используются?</summary>

portal позволяют дочерние ДОМ компонент вне узлов, где находится родительский компонент

отобразить модальное окно и тому подобное - для непересечения со стилями

```javascript
const PortalExampleApp = () => {}
```

</details>

<details>
  <summary>Для чего используются ref?</summary>

-   даёт возможность получить доступ к ДОМ узлам и react элементам (сторонних библиотек)
-   для хранения любых значений, изменение которых не должно вызывать рендер
-   для ссылки на элемент
-   императивный код нежелателен
-   для управления фокусом
-   для медиа контента

</details>

<details>
  <summary>Для чего нужны keys? Можно использовать index?</summary>

keys - строковый аттрибут, индетификатор элемента списка реакт компонентов

-   помогает реакту понять какие элементы были добавлены, изменены, удалены
-   только в крайних случаях = не стоит использовать index при проходе через map, если список может изменяться (т.е. если key=index может меняться для одного и того же элемента)

</details>

<details>
  <summary>Что такое render props? Для чего нужны?</summary>

render props - функция для шаринга между компонентами

-   будтоу слот во Vue

<!-- todo: code example -->
</details>

<details>
  <summary>Что такое state managment?</summary>

state managment - технология, позволяющая:

-   управлять состоянием приложения
-   отделять логику управления состоянием от UI компонентов (slices, thunks, etc.)
-   удобно передавать данные между дальними узлами

</details>

<details>
  <summary>Что такое redux?</summary>

state manager for js apps

-   хранить состояние в дереве в едином сторе
-   reducer должен быть чистой функцией, которая всегда возвращает новый объект состояния(===), т.к. ресурсоемкое сравнение вредит(==)

чтобы изменить стор надо отправить action через функцию dispatch()

```javascript
const ActionType = "AddCard"
const action = {
	type: ActionType,
	payload: {
		id: "123",
		text: "text",
	},
}

dispatch(action)
```

это action попадет reducer, где обработается определенным образом
он в аргументы он принимает state, action

```javascript
const reducer = (state = [], action) => {
    switch (action.type) {
        case ActionType:
            const newItem = {
                id: action.payload.id,
                text: action.payload.text,
            }
            return [..state, newItem]
    }
}
```

</details>

<details>
  <summary>Что такое action в redux?</summary>

action это обычный объект JS

с двумя полями:

-   type = название действия, которое action совершает
-   payload = данные, которые требуется к этому действию
</details>

<details>
  <summary>Как реализовать side effect в redux?</summary>

for ex: логгирование, отправка запроса на сервер

middleware - свой или redux thunk, redux saga

```javascript
const store = createStore(rootReducer, applyMiddleware(logger, thunk))
```

, где thunk из redux-thunk и logger - сайд еффект

```javascript
// свой middleware
const logger = (state) => (next) => (action) => {
	console.log("dispatching = ", action)
	console.log("state before = ", store.getState())
	const result = next(action)
	console.log("state after = ", store.getState())
	return result
}
```

```javascript
// thunk middleware
const getTodos = () => {
	return (dispatch) => {
		getAllTodos()
			.then((todos) => {
				dispatch({
					type: init_todos,
					payload: todos,
				})
			})
			.catch((err) => {
				console.error(err)
			})
	}
}
```

```javascript
// saga middleware
export function* loginFlow() {
	while (true) {
		const { payload } = yield take(LOGIN_REQUEST)
		const { username, password } = payload
		const task = yield fork(authorize, username, password)
	}
}
export function* loginFlowSaga() {
	yield loginFlow()
}
```

</details>

<details>
<summary>реакт реактивный?</summary>

-   react docs scheduling
-   реакт компоненты - функции
-   но мы их не запускаем
-   сам реакт вызывает их в нужные моменты
-   реакт рекурсивно обходит дерево
-   UI фреймворк шобы не было тормозов
-   реакт использует pull модель
-   реакт = планировщик, а чистой реактивности в нём нет
-   react batching добавлена в React 18 = планировка выполнения действия

</details>

<details>
<summary>Что такое Synthetics Event?</summary>

кроссбраузерная обёртка над нативными событиями\
интерфейс схож с нативными\
помогает всем событиям одинаково работать во всех бразуерах\
e.nativeEvent дает нативное событие\

</details>
