# Interview Questions about Redux

<details>
  <summary>Что такое state managment?</summary>

state managment - технология, позволяющая:

-   управлять состоянием приложения
-   отделять логику управления состоянием от UI компонентов (slices, thunks, etc.)
-   удобно передавать данные между дальними узлами

</details>

<details>
  <summary>Что такое Flux - архитектура? Какие сущности она имеет?</summary>

React отвечает за V или View в MVC.\
А как насчет M или Model части?\
Flux, шаблон проектирования, соответствует M в MVC

Компоненты архитектуры Flux взаимодействуют между собой скорее как EventBus и менее, чем MVC

Это архитектура, ответственная за создание слоя данных в JavaScript приложениях и разработку серверной стороны в веб-приложениях. Flux дополняет составные компоненты вида View в React, используя однонаправленный поток данных.

Flux-архитектура — архитектурный подход или набор шаблонов программирования для построения пользовательского интерфейса веб-приложений, сочетающийся с реактивным программированием и построенный на однонаправленных потоках данных.

flux-flow:
<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*3wIquwufaGTtdoqjN4Zudg.png" />

react-flux-flow:
<img src="https://miro.medium.com/v2/resize:fit:640/format:webp/1*ZCgdOM1n1uYwGZ1TN4UvuA.png" />

Основной отличительной особенностью Flux является односторонняя направленность передачи данных между компонентами Flux-архитектуры.
Архитектура накладывает ограничения на поток данных, в частности, исключая возможность обновления состояния компонентов самими собой.
Такой подход делает поток данных предсказуемым и позволяет легче проследить причины возможных ошибок в программном обеспечении.

В минимальном варианте Flux-архитектура может содержать три слоя, взаимодействующие по порядку:

-   Actions = Действия = выражение событий (часто для действий используются просто имена — строки, содержащие некоторый «глагол»). Новое действие не передаётся пока предыдущее полностью не обработано компонентами. Действия из-за работы источника действия, например, пользователя, поступают асинхронно, но их диспетчеризация явлется синхронным процессом. Кроме имени (`name` или же `type`), действия могут иметь полезную нагрузку (`payload`), содержащую относящиеся к действию данные.

-   Dispatcher = Диспетчер/Диспатчер = для передачи действий хранилищам. В диспетчере хранилища регистрируют свои функции обратного вызова (callback).

-   Store = Хранилище = место, где сосредоточено состояние (state) приложения. Остальные компоненты, согласно Flux, не имеют значимого (с точки зрения архитектуры) состояния. Изменение состояния хранилища происходит строго на основе данных действия и старого состояния хранилища при помощи чистых функций (reducer'ов).

-   View = Представление = React компонент, отвечающий за выдачу информации пользователю. Во Flux-архитектуре это — конечная точка потоков данных.

Dispatcher действует как реестр с зарегистрированными callback-ами, на которые отвечают Store. Store будут давать изменения, которые выбраны Controller-Views

Dispatcher может легко устанавливать зависимости между store-ами.

</details>

<details>
  <summary>Что такое redux?</summary>

state manager for js apps

1. UI отсылает Action через Dispatcher'а
2. Reducer обрабатывает полученный Action и изменяет Store
3. Store, содержит State, который и определяет UI
   цепочка замкнулась

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
  <summary>Когда следует использовать Reselect?</summary>

да... Redux довольно хрупкий в этом плане (при использовании useSelector() в купе с обычной функцией-селектором)

следует мемоизировать селекцию, если:

1. ты не знаешь стоит ли мемоизировать, а проект масштабнее около-среднего
2. если селектишь, используя другие селекторы
3. из селектора возвращаешь новую ссылку на переменную:
    - .filter, .map, и подобные методы работы с массивом, которые всегда создают новый массив
    - объект через литеральную нотацию
    - reduce, если считаем большой массив (спасёт проверка от reselect'а)

</details>

<details>
  <summary>Что и зачем batch?</summary>
 
НЕ НУЖЕН В РЕАКТЕ 18 ВЕРСИИ

для совершения нескольких экшенов в одно время

```js
function myThunk() {
	return (dispatch, getState) => {
		// will result in two re-renders
		dispatch(increment())
		dispatch(increment())
	}
}
```

будет таким:

```js
import { batch } from "react-redux"

function myThunk() {
	return (dispatch, getState) => {
		// should only result in one combined re-render, not two
		batch(() => {
			dispatch(increment())
			dispatch(increment())
		})
	}
}
```

</details>

<details>
  <summary>Отличия Redux и MobX?</summary>

MobX:

-   Несколько сторов
-   observables
-   неявноме изменение состояний - через подписки
-   меньше пустого boilerplate кода
-   паттерн наблюдателя = observer
-   ТИПА ТИПА тяжелее бандл
-   нет хороших devTools'ов, как раз из-за паттерна

Redux:

-   джс объекты для хранения
-   отслеживание измененй явное
-   Один стор
-   чистые функции для изменения состояний
-   архитектура Flux

</details>
