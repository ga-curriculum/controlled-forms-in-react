# ![[tktk Module Name] - tktk Microlesson Name](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

This will be a long microlesson going through the whole pattern of controlling a form's inputs using state.



Penultimate variant

```js
const [prop1, setProp1] = useState('');
const [prop2, setProp2] = useState('');

handleChangeProp1 = (e) => {
    setProp1(e.target.value)
} 

handleChangeProp2 = (e) => {
    setProp2(e.target.value)
} 

<input value={prop1} onChange={handleChangeProp1}>
<input value={prop2} onChange={handleChangeProp2}>
```

Eventually you get to something like this:
```js
const [newItem, setNewItem] = useState({
    prop1: '',
    prop2: ''
})

handleInputChange = (e) => {
    setNewItem({
        ...itemInState,
        [e.target.name] : e.target.value
    })
} 

<input value={newItem.prop} onChange={handleInputChange}>
```

You could cap it off with an onSubmit that just prints the object and preps students for sending that data somewhere else, but the goal is creating a form component with inputs managed by state and state hooks.