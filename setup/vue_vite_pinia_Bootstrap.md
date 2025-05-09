Setup a project

Draft

Best way use 
If you're starting a new project, you might find it easier to use the create-vue scaffolding tool, which creates a Vite-based project with the option to include Vue Router:

bash
yarn create vue



do  :  
 npm init vite@latest

it will prompt :  
 Project name : <add your name>  
 Select a framework : choose Vue  
 Select a variant : chosse Typescript  

It will scaffold new project


go into the project  
 - yarn install
 - yarn dev
project is now available

Let's add now the router and pinia
 - yarn add vue-router --save
 - yarn add pinia --save


# Add Bootstrap to the project 

 - Add boostrap
```shell
yarn add bootstrap
```

- import bootstrap to css  

To make the Bootstrap styles available, import the compiled CSS at the first line of your CSS entry file, main.css in /src/asset/:
```css
//main.css
@import 'bootstrap/dist/css/bootstrap.css';
```

- import Bootstrap with typescript  

DBootstrap's JavaScript plugins can be made available to the application using Vue's provide() and inject() functions. The provide() function allows to make data available at the app level, which can then be injected into components with the inject() function. Adjust the main.js file as follows
```typescript
//main.ts
import * as bootstrap from 'bootstrap';
import './assets/main.css'

import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'

const app = createApp(App)
app.provide('bootstrap', bootstrap);
app.use(createPinia())
app.use(router)

app.mount('#app')
```

- add example 

We gonna add a new component BSNav.vue inside src/component folder

```html
<template>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" aria-disabled="true">Disabled</a>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search"/>
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
  </template>
  
```

and latestly inside app.vue for testing

```html
<script setup>
import BSNav from '@/components/BSNav.vue';
</script>

<template>
  <div id="wrapper">
    <BSNav />
  </div>
</template>

<style scoped>
#wrapper {
  max-width: 600px;
  margin: 0 auto;
}
</style>
```

Reference : 
- [Get bootstrap official](https://getbootstrap.com/)
- [https://www.docs4.dev/posts/easy-bootstrap-5-integration-with-vue-3-step-by-step-guide](https://www.docs4.dev/posts/easy-bootstrap-5-integration-with-vue-3-step-by-step-guide)