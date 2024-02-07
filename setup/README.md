# ![[tktk Module Name] - Setup](./assets/hero.png)

## Setup

Open your Terminal application and navigate to your **`~/code/ga/lectures`** directory:

```bash
cd ~/code/ga/lectures
```

Run the command:

```bash
npm create vite@latest
```

You'll be prompted to provide a project name. `vite-project` is the default, but when you start typing that default will go away. Choose a name that makes sense for this project; in this case, we'll call the app `controlled-forms-in-react`.

Next, you'll select a framework. Use the arrow keys to choose the `React` option, and hit `Enter`.


Again, use the arrow keys to choose the `JavaScrip`t variant and hit `Enter`.


Move into the project you just created and run `npm i` to install the dependencies:


```bash
cd controlled-forms-in-react
npm i
```

Launch the app in VS Code:
```bash
code .
```

Open the `App.jsx` file in the src directory and replace the contents of it with the following:

```jsx
// src/App.jsx

const App = () => {

  return (
    <h1>Hello world!</h1>
  );
}

export default App
```