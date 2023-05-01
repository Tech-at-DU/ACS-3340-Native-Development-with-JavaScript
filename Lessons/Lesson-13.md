# ACS 3340 - Redux

Add Redux to a React Native project. 

## Objectives 

- Use React Native and Redux together
- Install and setup redux with react native
- Implement Redux with React Native

## Getting started 

Create a new React Native project:

```
npx create-expo-app react-native-redux
```

Navigate to the project: 

```
cd by-breed
```

Add dependencies: 

```
npm install @reduxjs/toolkit react-redux
```

You're all set! 

### Example

Setup some starter code. Add the store `app/store.js`:

```JS
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {},
})
```

Setup the Provider. Move everything out of `App.js` and put it in another component: `Main.js` in `App.js` add the following: 

```JS
import { store } from './app/store'
import { Provider } from 'react-redux'

import Main from './Main';

export default function App() {
  return (
    <Provider store={store}>
      <Main />
    </Provider>
  );
}
```

In `Main.js` add: 

```JS
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, SafeAreaView, Text } from 'react-native';

export default function Main() {
  return (
		<SafeAreaView style={styles.container}>
			<Text>Hello World</Text>
		</SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
    borderWidth: 1,
    borderColor: 'red'
  }
});
```

Here you have a your basic setup. 

### Add a slice 

Add a new file to handle a slice of state. I added `app/Todos.js`:

```JS
import { createSlice } from '@reduxjs/toolkit' // 1

const initialState = {
  value: ['Wash the Cat'],
}

export const todosSlice = createSlice({
	name: 'todos',
	initialState,
	reducers: {
		addTodo: (state, action) => {
			state.value.push(action.payload)
		}
	}
})

export const { addTodo } = todosSlice.actions
export default todosSlice.reducer
```

Edit `app/store.js`: 

```JS
import { configureStore } from '@reduxjs/toolkit'
import todosReducer from './todosSlice'

export const store = configureStore({
  reducer: {
		todos: todosReducer
	},
})
```

### Using Redux

Add a new component to add items to the todo list: 

```JS
import { StatusBar } from 'expo-status-bar';
import { StyleSheet, Text, View, TextInput, Button } from 'react-native';
import { useDispatch } from 'react-redux'
import { addTodo } from './app/todosSlice'
import { Provider } from 'react-redux'

export default function AddTodo() {
  const [name, setName] = useState("Hello")
  const dispatch = useDispatch()
  
  return (
		<View style={styles.container}>
			<TextInput 
				style={styles.textInput}
				value={name}
				onChangeText={setName}
			/>
			<Button 
				title='Add' 
				onPress={() => dispatch(addTodo(name))}
			/>
			<StatusBar style="auto" />
		</View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
		borderWidth: 1,
		borderColor: 'green',
		width: '100%',
		height: 30
  },
  textInput: {
    border: 1,
    borderWidth: 1,
    width: '80%',
    padding: 10
  }
});
```

Add this to `Main`. Import the component: 

```JS
import AddPassword from './AddPassword';
```

Add display the new component: 

```JS
<SafeAreaView style={styles.container}>

	<AddPassword />

	<StatusBar style="auto" />
</SafeAreaView>
```

List the todos. Make a new file `ListTodos.js`:

```JS
import { View, Text, FlatList, StyleSheet } from 'react-native'
import { useSelector } from 'react-redux'

export default function ListTodos() {
	const todos = useSelector(state => state.todos.value)
	return (
		<View style={styles.container}>
			<FlatList 
				data={todos}
				renderItem={({item, index}) => {
					return <Text>{item}</Text>
				}}
				keyExtractor={(item) => item}
			/>
		</View>
	)
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
		borderWidth: 1,
		borderColor: 'blue',
		width: '100%',
		height: 30
  },
  textInput: {
    border: 1,
    borderWidth: 1,
    width: '80%',
    padding: 10
  }
});
```

Add this component to `Main.js`. 

Import the component at the top: 

```JS
import ListTodos from './ListTodos';
```

Then display it below: 

```JS
<SafeAreaView style={styles.container}>
	<AddPassword />
	<ListTodos />
	<StatusBar style="auto" />
</SafeAreaView>
```

### Challenges 

 

## Resources 

- 





