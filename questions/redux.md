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

Flux-архитектура — архитектурный подход или набор шаблонов программирования для построения пользовательского интерфейса веб-приложений, сочетающийся с реактивным программированием и построенный на однонаправленных потоках данных.

Основной отличительной особенностью Flux является односторонняя направленность передачи данных между компонентами Flux-архитектуры. Архитектура накладывает ограничения на поток данных, в частности, исключая возможность обновления состояния компонентов самими собой. Такой подход делает поток данных предсказуемым и позволяет легче проследить причины возможных ошибок в программном обеспечении.

В минимальном варианте Flux-архитектура может содержать три слоя, взаимодействующие по порядку:

-   Действия (англ. actions) — выражение событий (часто для действий используются просто имена — строки, содержащие некоторый «глагол»). Диспетчеры передают действия нижележащим компонентам (хранилищам) по одному. Новое действие не передаётся пока предыдущее полностью не обработано компонентами. Действия из-за работы источника действия, например, пользователя, поступают асинхронно, но их диспетчеризация явлется синхронным процессом. Кроме имени (англ. name), действия могут иметь полезную нагрузку (англ. payload), содержащую относящиеся к действию данные.

-   Диспетчер/Диспатчер (англ. dispatcher) предназначен для передачи действий хранилищам. В упрощённом варианте диспетчер может вообще не выделяться, как единственный на всё приложение. В диспетчере хранилища регистрируют свои функции обратного вызова (callback) и зависимости между хранилищами.

-   Хранилище (англ. store) является местом, где сосредоточено состояние (англ. state) приложения. Остальные компоненты, согласно Flux, не имеют значимого (с точки зрения архитектуры) состояния. Изменение состояния хранилища происходит строго на основе данных действия и старого состояния хранилища при помощи чистых функций.

-   Представление (англ. view) — компонент, обычно отвечающий за выдачу информации пользователю. Во Flux-архитектуре, которая может технически не касаться внутреннего устройства представлений вообще, это — конечная точка потоков данных. Для информационной архитектуры важно только, что данные попадают в систему (то есть, обратно в хранилища) только через действия.
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
