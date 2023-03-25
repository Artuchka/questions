# Interview Questions about Redux

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
