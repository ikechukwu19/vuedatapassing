# How to pass data between vue js components

### Introduction

Due to the constantly growing complexities of modern web applications, the need
to break down the application into smaller, reusable and manageable chunks
called components keeps arising. The component-based architecture is a design
method where the software is divided into smaller, reusable units called
components which makes the entire development experience better, these
components need to share data to create functionality and interactivity

In this tutorial, we are going to learn how to pass data between vue js
components

## Prerequisites

To follow this tutorial you will need

- Basic knowledge of
  [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- Basic knowledge of [vue js](https://vuejs.org/)
- Node js and Node package manager (NPM)
  [installed](https://www.digitalocean.com/community/tutorial-series/how-to-install-node-js-and-create-a-local-development-environment)
  on your local or server environment
- A code editor installed on your development machine preferrably [Vs code]()

## Setting up vue application

we are going to build a todo application, the application's funtionality is
adding todos and toggling the todo status from completed to uncompleted. To get
started you are going to create a new vue application Create a new folder called
vuetodos

```sh
   $ mkdir vuetodos
```

To code above creates a new folder called vuetodos in your specified directory

Then create a new vue application by running

```sh
   $ npm create vue@latest
```

## Props

Props is a method of passing data in vue js from a parent to a child component,
the word props is a vue js keyword which stands for **Properties**, the data
flow in props is one way which is from parent to child this means that you
cannot pass data from child to parent using props

###### How to register props in vue components

with composition API

```js
<script setup>
  const props = defineProps(['foo']) console.log(props.foo)
</script>
```

`defineProps()` is a vue js macro that is used to register props to a component
