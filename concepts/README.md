# ![Controlled Forms in React - Concepts](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to differentiate between the real DOM and virtual DOM and understand the benefits of controlled form inputs in React.

## React's Virtual DOM

React's virtual DOM (VDOM) is part of what makes it so performant. It's helpful to understand how the virtual DOM is distinguished from the "real" DOM to better understand its role.

### Real DOM VS. Virtual DOM

The DOM is a representation of the HTML structure of a web application as nodes and objects. Put plainly, the DOM allows programming languages to affect an app's structure, style, and content. 

The virtual DOM, meanwhile, is an in-memory lightweight representation of the DOM that React creates and maintains. It's essentially a JavaScript object that mirrors the real DOM's structure. 

The actual DOM has no built-in methods for managing the application state, meaning that developers must handle the state manually. This can quickly become complex and increases the risk of desynchronization between the state of an application and what is being rendered to the user. 

The virtual DOM, on the other hand, is managed by React and is, therefore, inherently linked to the app's state. This means state management is simplified, with more consistency between the view and data. 

A virtual DOM leads to a more declarative approach to building a UI. We can tell React what state we want our page to be in, and it will make the "real" DOM match that state in a process called reconciliation. 

### A Helpful Analogy 

Imagine that the UI of our React app is a giant skyscraper. The real DOM is the building and represents the elements and structure displayed on the screen. Any changes we make to the building would be time-consuming and cumbersome. In fact, we'd need to tear down and rebuild the skyscraper whenever we wanted to make any changes to the building. Sounds expensive. 

React's virtual DOM is like a real-time holographic model of our skyscraper. Since it's just a holograph, we can easily manipulate and update. In fact, we can make frequent changes to our holograph without changing the actual building itself. When changes *do* need to be made to our building, the virtual DOM allows React to make only the necessary updates efficiently. Rather than tearing down and rebuilding, our skyscraper is compared against the holographic's new "ideal" model. It is only updated where React noticed any differences.  

## Controlled Form Inputs

In standard HTML, when a user types into a form input field, the browser internally updates the value of the input element. This value resides within the browser's memory and isn't directly accessible by our React component. This causes a disconnect between the UI and our app's state, which leads to two crucial issues:

- Our app can't react to changes: If our React component needs to respond to the input value (by displaying a greeting or performing validation, for example), it cannot know the latest value since it's not stored in state. 

- There's a one-way flow: The information only flows from the user to the browser, and our react component gets nothing. 

To solve this problem, we use [**controlled inputs**](https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable) to shift control from the browser to our React component. We can set up a controlled input using the following steps: 

First, we'll need to store the input value in our React component's state with `useState`, so let's import `useState`:

```jsx
import { useState } from 'react';
```

Next, we'll bind the input's `value` prop to the state variable:

```jsx
import { useState } from 'react';

const App = () => {

  const [nameInput, setNameInput] = useState('');

// ... rest of our component
}
```

- `nameInput` is the variable that will hold the value of the input field
- `useState('')` sets the initial value of the state variable to an empty string

Finally, we must attach an `onChange` event handler to the input. This captures the new value whenever a user types and updates state using `setState`:

```jsx
import { useState } from 'react';

const App = () => {

  const [nameInput, setNameInput] = useState('');

  const handleChange = (event) => {
    setNameInput(event.target.value);
  };

  return (
    <>
      <input
        type="text" 
        value={nameInput} // Bind state to value prop
        onChange={handleChange}  // Update state on change
      />
    </>
  );

};
```

- `value={nameInput}` ensures the input displays the current value from state
- `handleChange` is the event handler that captures changes in the input and updates the state using `setInputValue`

Now, our input is considered "controlled" by React! We'll explore the benefits of this setup next.
