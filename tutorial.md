# How to pass data between vue js components using props and emits

### Introduction

Due to the constantly growing complexities of modern web applications, the need to break down the application into smaller, reusable and manageable chunks called components keeps arising. The component-based architecture is a design method where the software is divided into smaller, reusable units called components which makes the entire development experience better, these components need to share data to create functionality,interactivity and improve reusablity

In this tutorial, we are going to learn how to pass data between vue js components using props and emits. 

## Prerequisites

To follow this tutorial you will need

- Basic knowledge of [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide), HTML and CSS
- Basic knowledge of [vue js](https://vuejs.org/)
- Node js and Node package manager (NPM) [installed](https://www.digitalocean.com/community/tutorial-series/how-to-install-node-js-and-create-a-local-development-environment) on your local or server environment
- A code editor installed on your development machine preferrably [Vs code]()

## Setting up vue application

To learn about `props` and `emits` in vue js we are going to build a todo application, the application's funtionality is adding todos and toggling the todo status from completed to uncompleted.

![completed project](https://res.cloudinary.com/silkydev/image/upload/v1728294703/articles/xrgwrslpwmw0wfzj3d7p.gif)
This is what the finished project would look like

To get started you are going to create a new vue application Create a new folder called vuetodos
```sh
  mkdir vuetodos
```

To code above creates a new folder called vuetodos in your specified directory

Then create a new vue application by running

```bash
  npm create vue@latest
```

The command above will install and excute create-vue which is the official vue project scaffolding tool. The command would trigger a prompt with vue js optional features which you may need in your project

The optional features would be presented as such in your command line

```bash
  ✔ Add TypeScript? … No / Yes
  ✔ Add JSX Support? … No / Yes
  ✔ Add Vue Router for Single Page Application development? … No / Yes
  ✔ Add Pinia for state management? … No / Yes
  ✔ Add Vitest for Unit testing? … No / Yes
  ✔ Add an End-to-End Testing Solution? … No / Cypress / Nightwatch / Playwright
  ✔ Add ESLint for code quality? … No / Yes
  ✔ Add Prettier for code formatting? … No / Yes
  ✔ Add Vue DevTools 7 extension for debugging? (experimental) … No / Yes
```

The options needed for this tutorial are vue router and pinia for state management. You can select `No` for others

Then run the command below to install all the dependencies

```bash
 npm install
```

You can clone the completed project from this github [repo]()

## Props

Props is a method of passing data in vue js from a parent to a child component. The word props is a vue js keyword which stands for **Properties**, the data flow in props is one way which is from parent to child this means that you cannot pass data from child to parent using props, props are ready only; the data cannot be modified by the child component

###### How to register props in vue components

with composition API

```js
<script setup>
  const props = defineProps(['foo'])  
  console.log(props.foo)
</script>
```

`defineProps()` is a vue js macro that is used to register props to a component, `foo` is the prop name that was passed.

with opitions API

```js
export default {
  props: ["foo"],
  created() {
    console.log(this.foo);
  },
};
```

In options API the props are exposed on `this` keyword, example `this.foo` to access the foo props above.

In your _vuetodos_ project go to `src/components` and create two components `Todos.vue` and `AddTodo.vue`. Inside `Todos.vue` paste the code below

```js
<script setup>
const props = defineProps({
  todos: {
    type: Array, // prop type
    required: true,
  },
  activeTodos: {
    type: Number, // prop type
    required: true,
    default: 0, // default value of prop
  },
});
</script>
```

From the code above, the `defineProps()` macro takes in an object which contains two props `todos` and `activeTodos`. The `todos` prop is an array of our todos and has the required property set to `true` this means that the code would throw an error when the `todos` prop is not passed. The `activeTodos` prop shows us the number of active todos that we have left this defaults to 0

```html
<template>
  <div class="todo_wrapper">
    <h3>Active Todos {{ activeTodos }}</h3>
    <div
      class="todo"
      v-for="todo in todos"
      :class="todo.completed ? 'completed' : ''"
      :key="todo.id"
    >
      <input
        type="checkbox"
        name="check"
        :id="todo.id"
        :checked="todo.completed"
      />
      <label :for="todo.id"> {{ todo.title }} </label>
    </div>
  </div>
</template>
```

This is the template part of the code. The `activeTodos` prop is rendered on the template use the vue.js mustache syntax `{{ }}`. The `v-for` directive is used to loop through the `todos` prop. `:class="todo.completed ? 'completed' : ''"` this is a dynamic class binding that applies if the completed property of the todo is `true`. We have a checkbox input field that can be use to toggle the status of the todo

```css
<style>
.todo_wrapper {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 16px;
}

.completed {
  text-decoration: line-through;
  color: rgba(0, 0, 0, 0.212);
}


.todo {
  display: flex;
  align-items: center;
  gap: 8px;
}

label {
  cursor: pointer;
}

</style>
```

The next step is importing `Todo.vue` component into `App.vue`. In this case `App.vue` is the parent component which would pass the todos data to the child component `Todo.vue`

```js
<script setup>
import Todos from "@/components/Todos.vue";
import { ref, computed } from "vue";


const todos = ref([
  {
    id: 1,
    title: "Do laundry",
    completed: false,
  },
  {
    id: 2,
    title: "Read a book",
    completed: false,
  },
  {
    id: 3,
    title: "Go to gym",
    completed: false,
  },
  {
    id: 4,
    title: "Visit a friend",
    completed: true,
  },
]);


// computed function that gets the length of active todos
const activeTodos = computed(() => {
  const active = todos.value.filter((todo) => todo.completed === false);
  return active.length;
});
</script>
```

First you import the `Todos.vue` component also import vue `ref` for reactive data and the `computed` property that automatically tracks the todos reactive data. The `computed` function tracks the todos to get the length of uncompleted todos

The template part of the code would look like this

```html
<template>
  <main>
    <h2>Todos</h2>
    <div class="wrapper">
      <Todos :todos="todos" :activeTodos="activeTodos" />
    </div>
  </main>
</template>
```

A `main` tag that wraps around the whole element just like a container. A level 2 heading for the title and a wrapper class for for the todos. To pass the todos data as props into the todos component you dynamically bind the data to the todo props you define earlier on `Todo.vue` component using the `v-bind` directive or the shortcut syntax `:` which is the `:todos="todos"` and the same for the active todos `:activeTodos="activeTodos"`

## Emits

Emit is a method of sending custom events from child component to parent component. Unlike `props` the emit method sends data from child to parent. The emits method is decleared on the template using `$emit('event')`

```html
<template>
  <button @click="$emit('event')">submit</button>
</template>
```

or on the script using a built in macro `defineEmits(['event'])`

```js
<script setup>
  const emit = defineEmits(['customEvent']) 
  function handleEvent(){
    emit("customEvent")
  }
</script>
```

Then it is listened on the parent component, directly where the child component is decleared.

```html
<template>
  <ChildComponent @customEvent="() => console.log('event recieved')" />
</template>
```

The event is listened to using the shorthand `@` then the name of the emitted event, a callback is passed to handle the event. The callback can be passed directly on the template as the example above or a seperate function can be created to handle it.

```html
<template>
  <ChildComponent @customEvent="eventHandler" />
</template>
```

```js
<script setup>
  function eventHandler() {
    console.log("event recieved!")
  }
</script>
```

To emit a specific value with an event the value is passed as a second argument to the `$emit` method. `$emit('event', value)`

```html
<template>
  <button @click="$emit('event', 1)">submit</button>
</template>
```

and then it is caught on parent in two different ways one is using an arrow function on the template

```html
<ChildComponent @event="(value)=> console.log(value)"></ChildComponent>
```

The value here is the second argument that was passed on the emit method on the child component, it could be called anything. Then for the second way of catching an emitted value

```html
<template>
  <ChildComponent @customEvent="eventHandler" />
</template>
```

```js
<script setup>
function eventHandler(value) {
  console.log(value)
}
</script>
```

In your _vuetodos_ project go to `src/components`open the `AddTodos.vue` component you created earlier and paste the following codes below

```js
<script setup>
import { ref } from "vue";
const newTodo = ref("");
const emits = defineEmits(["addTodo"]);

function addTodo() {
  emits("addTodo", newTodo.value);
  newTodo.value = "";
}
</script>
```

We have the `newTodo` reactive data which is the todo data that would be sent to the parent component then the we are emitting `addTodo` function with the `newTodo` as an argument, after the emit the `newTodo` data is set back to an empty string

```html
<template>
  <form @submit.prevent="addTodo">
    <input type="text" placeholder="Enter todo" v-model="newTodo" />
    <button>Add Todo</button>
  </form>
</template>
```

For the template part of the code we have a form that calls the `addTodo` function when submitted, there is an [event modifier](https://vuejs.org/guide/essentials/event-handling#event-modifiers) `.prevent` which is the same as vanila javascript `event.preventDefault()` this stops the the default behaviour of a form submission. The input binds to the `newTodo` data using the `v-model` directive

```css
<style>
form {
  padding: 16px;
  border-bottom: 1px solid #333;
  display: flex;
  align-items: center;
  gap: 8px;
}

input {
  border: 1px solid #262626;
  padding: 8px;
}

button {
  border: none;
  padding: 8px;
  background-color: black;
  color: white;
}
</style>
```

This is the styling for the `addTodo` component

In the parent component which is `App.vue` you would import the `AddTodos.vue` component 

```js
<script setup>
import { ref, computed } from "vue";
import Todos from "@/components/Todos.vue";
import AddTodo from "@/components/AddTodo.vue"; // AddTodo import

const newTodo = ref("");
const todos = ref([
  {
    id: 1,
    title: "Do laundry",
    completed: false,
  },
  {
    id: 2,
    title: "Read a book",
    completed: false,
  },
  {
    id: 3,
    title: "Go to gym",
    completed: false,
  },
  {
    id: 4,
    title: "Visit a friend",
    completed: true,
  },
]);

const activeTodos = computed(() => {
  const active = todos.value.filter((todo) => todo.completed === false);
  return active.length;
});

// Add todo function
function addTodo(data) {
  if (data === "") return window.alert("Todo cannot be empty");
  todos.value.push({
    id: todos.value.length + 1,
    title: data,
    completed: false,
  });
  newTodo.value = "";
}
</script>
```

The `addTodo` function is the callback of the emitted function, The first part of the function is the make sure that the todo is not an empty string, when this passes we add the data to the todos array. For the `id` of the todo we take the length of the todos data and increase it by one, the `completed` is set to `false` by default  

```html
<template>
  <main>
    <h2>Todos</h2>
    <div class="wrapper">
      <AddTodo @addTodo="addTodo" /> // Add todo component
      <Todos
        v-bind:todos="todos"
        :activeTodos="activeTodos"
      />
    </div>
  </main>
</template>
```
The next place that the emit method is needed in this project is when toggling the completed status of a todo. To do that we would go back to the `Todos.vue` component and emit a toggle event when the checkbox is checked or unchecked using HTML's change event

```html
<template>
  <div class="todo_wrapper">
    <h3>Active Todos {{ activeTodos }}</h3>
    <div
      class="todo"
      :class="todo.completed ? 'completed' : ''"
      v-for="todo in todos"
      :key="todo.id"
    >
      <input
        type="checkbox"
        name="check"
        :id="todo.id"
        @change="$emit('toggle', todo.id)" 
        :checked="todo.completed"
      />
      <label :for="todo.id">
        {{ todo.title }}
      </label>
    </div>
  </div>
</template>
```

When emitting the `toggle` event we pass along the todo id. On the parent component `App.vue` where we listen to the `toggle` event we would use the todo id to identify the particular todowe want to change

```html
<template>
  <main>
    <h2>Todos</h2>
    <div class="wrapper">
      <AddTodo @addTodo="addTodo" />
      <Todos
        v-bind:todos="todos"
        :activeTodos="activeTodos"
        @toggle="toggleTodo"
      />
    </div>
  </main>
</template>
```

```js
<script setup>
import { ref, computed } from "vue";
import Todos from "@/components/Todos.vue";
import AddTodo from "@/components/AddTodo.vue";

const newTodo = ref("");
const todos = ref([
  {
    id: 1,
    title: "Do laundry",
    completed: false,
  },
  {
    id: 2,
    title: "Read a book",
    completed: false,
  },
  {
    id: 3,
    title: "Go to gym",
    completed: false,
  },
  {
    id: 4,
    title: "Visit a friend",
    completed: true,
  },
]);

const activeTodos = computed(() => {
  const active = todos.value.filter((todo) => todo.completed === false);
  return active.length;
});


function addTodo(data) {
  if (data === "") return window.alert("Todo cannot be empty");
  todos.value.push({
    id: todos.value.length + 1,
    title: data,
    completed: false,
  });
  newTodo.value = "";
}

// toggle todo function
function toggleTodo(id) {
  todos.value.forEach((todo) => {
    if (todo.id === id) {
      todo.completed = !todo.completed;
    }
  });
}
</script>
```
The `toggleTodo` iterates through the todos array using the `forEach` method, with this we look for todo with the id that matches the id that we are passing we then change the status to opposite of the current status.

## Conclusion
In this article you learnt how to pass data between between vue js components via props and emits by building a todo app. You also learnt about the component based architecture and it's importance

There are other ways vue js components communicate or pass data between each other examples are provide/inject, states and slot. You can learn more about them from the vue js official documentation