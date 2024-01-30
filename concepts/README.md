# ![[tktk Module Name] - Concepts](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

CREATOR GUIDANCE

Main concept is here is that react wants to be in charge of everything using its virtual DOM, state, and so on. Nothing should be getting pulled directly from the real DOM, so what do we do about commonly pulled stuff like input values?

Answer: store them in state and use eventhandlers to consistently update state onChange of any form input changing. This is known as "Controlled inputs" and you can link directly to react docs if you'd like: https://react.dev/reference/react-dom/components/input#controlling-an-input-with-a-state-variable

