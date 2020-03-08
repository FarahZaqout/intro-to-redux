# intro-to-redux

The Absolute beginner's guide to getting started with redux and react-redux.

## Part 1: Redux

## What is redux?

Redux is a state management library. It stores all the app state in one place and makes it easy to avoid complications of shared state.

Note that redux is not a react exclusive library. It can be used with anything. Even pure javascript. [Look at this example later](https://github.com/reduxjs/redux/blob/master/examples/counter-vanilla/index.html).

## The redux flow:

In redux, data still flows in one direction. The redux cycle is identical in all elements regardless of whether they're nested or not. This makes the logic of managing state much easier to understand and implement.

Let's keep away from code for now.

### Key words:

Don't worry if these words are confusing.

- `store` [object]: a palce to hold our state.
- `reducer` [function]: a way to inform the store we want to do somethig with the state. We are not allowed to talk to the store without a reducer. Remember this one because it's important. **Store only talks with reducers. ONLY.**
- `action` [object]: This object is what we use to specify how we want to change our state.
- `dispatch` [function]: a way to tell the reducer to perform an `action`.

![Redux Flow](https://i.imgur.com/fWAGTuu.png)

## Concepts to Code:

This still sounds vague and confusing. So let's see what each of the terms above means and how it might look like in code.

- create a new react project: `npx create-react-app my-first-redux`
- `cd my-first-redux`
- install redux: `npm i redux`

### The Redux State (and store):

We said that redux is a state management library. In order to manage a state, we first need to have a state.

In our `App.js` let's create our redux state.

```javascript=
// App.js
const reduxState = {
	name: 'Farah',
	age: 300
};
```

In redux, we cannot interact with the state without the store (see the picture above). So let's create our redux store and place the state inside of it.

```javascript=
// App.js
import { createStore } from 'redux';

const reduxState = {
	name: 'Farah',
	age: 300
};

const store = createStore(reduxState);
```

- run the project: `npm start`

**_This code will throw an error "expected reducer to be a function"._**
![spook](https://i.imgur.com/pA320Pb.png)

As we said earlier we may not interact with the store without a `reducer`. Even to set its `initial state`. **_`Store` only talks with a `reducer`_**.

### Our First Reducer:

Reducers are functions. The store runs the `reducer` and returns a `new state` based on the `reducer`.

Okay so our `store` does not want an object. It wants a function that returns that object. Let's do it!

```javascript=
// App.js
import { createStore } from 'redux';

const reduxState = {
	name: 'Farah',
	age: 300
};

// function with default param state.
const reducer = (state = reduxState) => {
	return state;
};

const store = createStore(reducer);
```

---

### Store is not the state:

Now that the error is gone, let's `console.log(store)` and see what it returns.

The console output is a huge object we don't care about it, for now. Let's try something else then. `console.log(store.getState())`

Change the return value of the reducer above and see if `store.getState()` reflects that change.

Yay! We have created a redux state!

### Cleaning Up:

We can't have our `state management` logic and our main `App` logic in the same file. We have to split our code to keep it clean...and to be able to understand the next steps, too.

- Create a `redux` directory.
- create an `index.js`
- inside of `redux`, create a `store` directory.
- move the redux code into it and export it.

```javascript=
import { createStore } from 'redux';

const reduxState = {
	name: 'Farah',
	age: 300
};

// this will take the reduxState above as default parameters and then return it.
const reducer = (state = reduxState) => {
	return state;
};

const store = createStore(reducer);

export default store;
```

#### File structure: (`>` for directories, `-` for files)

```
 > redux
      > store
          -- index.js (exports store to redux/index.js)
      -- index.js (exports the store to the App.js)
```

### Changing Values of the state:

Now that we have created our state, we need a way for it to change, otherwise we can't really do much with it.

### _Enter Actions!_ :sunglasses:

![action](https://i.imgur.com/uuJKfb1.jpg)

An action is a signal for a reducer to `act` and tell the store to do something with the state. For example,

```javascript=
// store/index.js
const addHeight = {
	type: 'ADD_HEIGHT',
	payload: '420 cm'
};

// this line must be below the createStore function.
store.dispatch(addHeight);
```

In order to use actions, we need to `dispatch` them. By using the `store.dispatch(action)` as above.

There are two things to know about actions:

- the `type`
- the `payload`

> The type is a mandatory key for actions. We use the action `type` to know which action we are using. Think of it like a `user_id`. It must be unique.

> the payload is what we will tell the reducer to use for the new state. We also need to tell the reducer what exactly to do with it.

Now that we used `dispatch` Let's see what happens...**AAAAAAAAAAAAAAAAAAAAAND IT'S NOTHING...** :sunglasses::sunglasses::sunglasses:
**WAIT WHAT?** Let's review.

![Redux Flow](https://i.imgur.com/fWAGTuu.png)

A reducer is responsible for returning the new state. Even if we use `dispatch`, we still need to tell the reducer what to do with the action.

let us get back to reducers. This is the reducer from our store directory.

```javascript=
// store/index.js
const reduxState = {
	name: 'Farah',
	age: 300
};

const reducer = (state = reduxState) => {
	return state;
};
```

- Add a new parameter `action` to the reducer.
- Log it in the console and see the result.

```javascript=
const reduxState = {
	name: 'Farah',
	age: 300
};

const reducer = (state = reduxState, action) => {
	console.log('this is the action', action);
	return state;
};
```

We did not give a value to the action. So it should be `undefined` from the console output. Right? **RIGHT?** But it's not.

The console will output something like this: `{type: "ADD_HEIGHT", payload: "420 cm"}`
...
<br/>

![Kat williams](https://i.imgur.com/8Gz8XCt.jpg)

---

The reducer _already knows_ it is responsible for taking actions and changing the state, so its first argument is always reserved for the `state`, and the second argument is reserved for the `action`.

**_Once we dispatch an action, we can use it directly in the reducer as second argument._**

So the reason `store.dispatch(addHeight)` didn't work was not because the reducer didn't see it. It's because the reducer has no idea what we want to do with it.

Let's `dispatch` another action, for style points!

```javascript=
// store/index.js
const addWeight = {
	type: 'ADD_WEIGHT',
	payload: '420 kg'
};

store.dispatch(addWeight);
```

now let's see what the `console` output gives us.

```
this is the action {type: "@@redux/INIT5.5.l.d.i.3.p"}
this is the action {type: "ADD_HEIGHT", payload: "420 cm"}
this is the action {type: "ADD_WEIGHT", payload: "420 kg"}
```

Every time we dispatch an action the reducer sees it. It sees all actions at the same time. So to use these actions, we can just tell the reducer how to `change the state` based on the `action type`.

```javascript=
// store
const reducer = (state = reduxState, action) => {
	// we return the new state based on the action type.
	if (action.type === 'ADD_HEIGHT') {
		return { height: action.payload };
	} else {
		return state;
	}
};
```

This should do the trick. Let's look at the new state in `localhost:3000`.

It should just add `height` to the state. Instead, it removed the old state and relaced it entirely with the new one...help!

#### <span style="text-decoration:line-through"> Superman</span> Spread Operator to the Rescue:

```javascript=
// store
const reducer = (state = reduxState, action) => {
	if (action.type === 'ADD_HEIGHT') {
		return {
			...state,
			height: action.payload
		};
	} else {
		return state;
	}
};
```

If you don't understand what happened, read up on [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax).

## 10-Minute Break

Now that we've successfully created and updated our state, we can take a triumphant break!

---

## Clean up:

Let's refactor and split the code to make it easier to scale up.

### First: Refactoring Actions!

Right now, actions will add the same value to the state every time. So `addHeight` will always add `'420 cm'` to the state. This is not very useful. We want it to add any possible height.

This is where **`action creators`** come into play.

#### Action Creators:

Action creators are simple. We take an action (object), and put it inside a function. This allows us to change the value of the `action.payload` as needed!

```javascript=
// we refactor the action object to an action creator function
// this function will return the old action...kind of.
const addHeight = (payload) => {
	return {
		type: 'ADD_HEIGHT',
		payload
	};
};

store.dispatch(addHeight('420 cm'));
```

Now we can set the payoad to have dynamic value.

Do the same with `addWeight` and allow us to change the weight attribute in the state using the reducer.

### Second: Split the code!

1- Separate actions logic from store logic.

- create a directory `actions`.
- Move the actions `addWeight` and `addHeight` to the directory.

2- Separate reducer logic from store logic.

- create a directory `reducers` (yes there will be more reducers).
- move the reducer function to the `reducer` directory.

your `store` directory should have an `index.js` and look somewhat like this.

```javascript=
import { createStore } from 'redux';
import { addHeight, addWeight } from '../actions';
import reducer from '../reducers';

const store = createStore(reducer);

// for now we will keep these lines just to make sure stuff work.
store.dispatch(addHeight('420 cm'));
store.dispatch(addWeight('3300 kg'));
console.log(store);

export default store;
```

Inside your `src` directory, redux should be structured like this:

```
 > redux
   > store
   > actions
   > reducers
   - index.js (imports the store and exports it to app)
```

## Recap:

- Redux is a state management library. It stores the entire state of our application in one place.
- The flow of redux is as follows:
  ![Redux Flow](https://i.imgur.com/fWAGTuu.png)
- We can only access the state from the redux `store`.
- The only thing allowed to change the state is a function `reducer`.
- A reducer is a function that already knows it should receive the `old state` and`actions` then return a `new state`.
- We pass `actions` to the reducer via the `store.dispatch` method.
- Actions are objects with `type` and `payload`

## Challenge: Pair Program a Redux State

Now, try to implement the following state in redux -no peeking at the code snippits! Look at the recap points if you want.

- create a new redux app `npx create-react-app my-second-redux`
- `cd my-second-redux`
- `npm i redux`

### Requirements:

- `store.getState()` should return the following object:

```javascript=
{
    user: { name, age },
    location: { country, city },
    favoriteFood: [item1, item2, item3]
}
```

- Store, actions and reducer are all separated from each other.

---

## 10-Minute Break

Alright, you've earned a break. Get a coffee and some fresh air, then come back.

---

## Part 2: Connecting React with Redux.

Wait, so all this has been just doing redux without react? Yes. We still need to set up redux to work with react.

Now since the state only exists in one place, all our components don't have their own state anymore. They only use props, and get their state directly from redux.

### Before we dive in:

Before we can connect react to redux, we need some react components. So let's get at least one component to work.

- create a `components` directory
- create a Form directory

```javascript=
class Form extends Component {
	render() {
		return (
			<form>
				<label htmlFor="input">Input</label>
				<input id="input" type="text" />
				<input type="submit" value="submit" />
			</form>
		);
	}
}
```

- Let's add an onChange and onSubmit to the form.

```javascript=
class Form extends Component {
	onInputChange = (e) => {
		console.log(e.target.value);
	};

	onSubmit = (e) => {
		// we will be using this later
		e.preventDefault();
	};

	render() {
		return (
			<form>
				<label htmlFor="input">Input</label>
				<input onChange={this.onInputChange} id="input" type="text" />
				<input type="submit" value="submit" />
			</form>
		);
	}
}
```

- Run the app and see if the `onInputChange` function works.
- Now let us get into `react-redux`

### Key Terms:

- **Provider** [component]:
  - Wraps our entire app component and allows it to see the redux store.
- **mapStateToProps** [function]:
  - This function is custom. We have to create it.
  - Links the `redux state` to individual component `props` and allows components to see its values.
- **mapDispatchToProps** [function]:
  - This function is custom. We have to create it.
  - Opposite of `mapStateToProps`, allows individual `react` components to `dispatch` actions to the `redux store`.
- **connect** [function]:
  - This function is from `react-redux`
  - Allows us to link `mapStateToProps` and `mapDispatchToProps` with our component so the component can use them.

## Concepts to Code: Round 2!

To connect react to redux we need to use the library `react-redux`

### Getting Started:

#### Provider

First thing we do is we wrap our `App` component with the react-redux `Provider`, to make sure we can give all component access to the redux store.

```jsx
import { Provider } from 'react-redux';

function App() {
	return (
		<Provider store={store}>
			<div className="App">
				<Form />
			</div>
		</Provider>
	);
}
```

Now that we have the provider exposing the store to our App, we can start working on the `Form` component itself.

### Task 1: Granting Access to the State.

#### mapStateToProps:

The `mapStateToProps` function takes the `state` from our store as argument, and then returns an object. In that object, we can extract the values we need.

```javascript=
// components/Form/index.js
const mapStateToProps = (state) => {
	return {
		height: state.height
	};
};
```

So how does redux know to use this function if we just wrote it? This is where the `connect` function comes in.

#### Using connect:

So how do we `connect` the redux state to our componenet? Simple. We use the connect function as follows:

```javascript=
// components/Form/index.js
import { connect } from 'react-redux';

// Instead of exporting the Form directly
// We export the connect function.
export default connect(mapStateToProps)(Form);
```

Now we can easily access the `Height` via `props`. All we will do now is add `console.log(this.props.height)` in the render function of our `Form` component to see that we have access to it. We can put it into an element, send it to server (more on that later), or do whatever we want with it.

---

### Challenge: Allowing our component to dispatch actions

Take 10 minutes to look up and implement the `mapDispatchToProps` function. Do it in pairs.

- Allow the form component to dispatch the `addHeight` action with new values based on user input.

---

## Lunch Break

---

## Pair Project: Todo App (1-2 days)

If you understand how the things above work, you know more about redux than I did a week into learning it!

#### Now let's Build a simple todo app with the following functionality:

- Add Todo
- Delete Todo
- Edit Todo
- Mark Todo as done.

Stretch Goal:

- Sort Todos

**_if you finish early, go around the room, see if others are stuck or having trouble with something, try and support them_**

---

## Part 3: Async actions (day 3)

> > TODO: add a fetch code snippet to break redux.

![This is Fine](https://i.imgur.com/oUolWiL.jpg)

### Redux Thunk
