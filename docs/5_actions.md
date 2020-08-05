# 5. Introducing React Hooks

We are finally ready to make the components interactive and responsive to user input.

For this project, we will be adding actions on three components:

Component | User input | Response
--------- | ---------- | --------
`ListItem` | Click | Remove the `listItem` from the `List`
Delete Icon | Click | Remove the `listItem` from the `List`
Button | Click | Add a `listItem` with a `TextField` where users can input text
TextField* | Enter | Add the user input into the `List`

*Only shows when user click on the `Button`.

## 5.1 Concepts

Before we start interacting the components we need to learn about [React Hooks](https://reactjs.org/docs/hooks-intro.html).

**What is a Hook?** A Hook is a special function that lets you “hook into” React features. For example, `useState` is a Hook that lets you add React state to function components. 

**When would I use a Hook?** If you write a function component and realize you need to add some state to it, previously you had to convert it to a class. Now you can use a Hook inside the existing function component. We’re going to do that right now!

!!! question "What is a function component?"
	```JS
	function Example(props) {
 	 // You can use Hooks here!
  	return <div />;
	}
	```
	The code above is a **function component**. They are previously called Stateless components but we can now write functions and states inside them. You can only use Hooks in function components.

### 5.1.1 `UseState`

!!! info
	This section is modified from [https://reactjs.org/docs/hooks-overview.html#state-hook](https://reactjs.org/docs/hooks-overview.html#state-hook)

This example renders a counter. When you click the button, it increments the value:

```JS
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Here, `useState` is a *Hook* (we'll talk about what this means in a moment). We call it inside a function component to add some local state to it. React will preserve this state between re-renders.

`useState` returns a pair: 

+ the *current* state value (i.e. `count`), and
+ function that lets you update it (i.e. `setCount`). You can call this function from an event handler or somewhere else.

!!! tip
	It is convention to name the function starting with `set`, e.g. `setCount`

The only argument to `useState` is the initial state. In the example above, it is `0` because our counter starts from zero.  The initial state argument is only used during the first render.

#### Declaring multiple state variables

You can use the State Hook more than once in a single component:

```js
function ExampleWithManyStates() {
  // Declare multiple state variables!
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

The [array destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Array_destructuring) syntax lets us give different names to the state variables we declared by calling `useState`. These names aren't a part of the `useState` API. Instead, React assumes that if you call `useState` many times, you do it in the same order during every render. We'll come back to why this works and when this is useful later.

### 5.1.2 Effect Hook

You've likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations "side effects" (or "effects" for short) because they can affect other components and can't be done during rendering.

The Effect Hook, `useEffect`, adds the ability to perform side effects from a function component.

For example, this component sets the document title after React updates the DOM:

```JS
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

When you call `useEffect`, you're telling React to run your "effect" function after flushing changes to the DOM. Effects are declared inside the component so they have access to its props and state. By default, React runs the effects after every render -- *including* the first render. (We'll talk more about how this compares to class lifecycles in [Using the Effect Hook](/docs/hooks-effect.html).)

Effects may also optionally specify how to "clean up" after them by returning a function. For example, this component uses an effect to subscribe to a friend's online status, and cleans up by unsubscribing from it:

```JS
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

In this example, React would unsubscribe from our `ChatAPI` when the component unmounts, as well as before re-running the effect due to a subsequent render. (If you want, there's a way to [tell React to skip re-subscribing](/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) if the `props.friend.id` we passed to `ChatAPI` didn’t change.)

Just like with `useState`, you can use more than a single effect in a component:

```JS
function FriendStatusWithCounter(props) {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  const [isOnline, setIsOnline] = useState(null);
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }
  // ...
```

### 5.1.3 Rules of Hooks

Hooks are JavaScript functions, but they impose two additional rules:

* Only call Hooks **at the top level**. Don’t call Hooks inside loops, conditions, or nested functions.
* Only call Hooks **from React function components**. Don’t call Hooks from regular JavaScript functions.

We provide a [linter plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) to enforce these rules automatically. We understand these rules might seem limiting or confusing at first, but they are essential to making Hooks work well.

## 5.2 Adding `useState` hook

We need the application to watch over the state for two components:

1. The items in the list
2. The textbox that appears when the user clicks on the "Add Item" button.

First import `useState` from React and `TextField` from `@material-ui/core`:

```JS
import React, {useState} from "react";
```

Remove the `listItems` variable you initially declared and add the following lines of code below the function `App`:

```JS
function App() {
	const [listItems, setListItems] = useState(["Cockles", "Hum", "Mee Siam"]);
	// Initial items in the list

	const [textBox, setTextBox] = useState(false);
	// Initial state of the textbox is disabled
	// code omitted for brevity
}
```
## 5.3 Configuring the button

When the user clicks on the "Add Item" button, we need to:

1. Change the state of `textBox` to `true`,
2. Display a textbox as a `listItem` at the bottom of the List.

First, change the code for the `Button` component to add an `onClick` prop:

```JS
<Button onClick={() => setTextBox(true)} size="small">
	Add Item
</Button>
```

Next, add the following code block just before the `</List>` tag:
```JSX
{textBox ? (
	<div>
	<ListItem>
		<ListItemIcon>
		<ShoppingCartIcon />
		</ListItemIcon>
		<TextField
		autoFocus
		label="Enter item"
		style={{ width: "100%" }}
		/>
	</ListItem>
	<Typography style={{ marginLeft: 70 }} variant="subtitle1">
		Press Enter to add{" "}
	</Typography>
	</div>
) : null}
```

This code conditionally renders the textbox if the state of `textBox` is set to `true`. Else it renders nothing.

## 5.4 Configuring the `TextBox`

Next, we need to add an event listener to the textbox such that when the user presses Enter, it automatically adds the user's input as a new item in `listItems`.

First we need to create a function to append the user's input into `listItems`. Add the following code **before** the function `App`:
```JS
  function appendItem(item) {
    setListItems(listItems.concat(item));
    setTextBox(false);
  }
```

!!! warning
	You can only modify the state of `listItems` and `textBox` with `setListItems` and `setTextBox` respectively. Do not modify the variable directly.


Add the `onKeyPress` prop to the `TextField` component:

```JSX
<TextField
	autoFocus
	label="Enter item"
	style={{ width: "100%" }}
	onKeyPress={e =>
		e.key === "Enter" ? appendItem(e.target.value) : null
	}
/>
```

## 5.5 Configuring the delete icon button

Finally, we need to handle the event where the user clicks on the dustbin icon such that it would delete the item from the list.

Add the `onClick` prop to `DeleteForeverIcon`:

```JS
<IconButton aria-label="delete">
	<DeleteForeverIcon
		onClick={() => removeItem(item)}
		style={{ color: "red" }}
	/>
</IconButton>
```

## You are done!

Test out your app. Does it work like the one below?

<iframe src="https://codesandbox.io/embed/lta-devops-immersion-programme-react-web-app-demo-answer-zzocc?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="LTA DevOps Immersion Programme - React Web App Demo (Answer)"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

Have any comments or feedback? Contact [me](mailto:chiam_da_jian@lta.gov.sg)!