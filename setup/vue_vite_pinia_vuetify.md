Setup a project


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


# Add vuetify to the project 
Into the existing project do the following command

```shell
> yarn add vuetify
```

and change the main.ts file as the following
```typescript
import { createApp } from 'vue'

// Vuetify
import 'vuetify/styles'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'

// Components
import App from './App.vue'

const vuetify = createVuetify({
  components,
  directives,
})

createApp(App).use(vuetify).mount('#app')
```

To check that all works change the content of app.vue to reflect this :
```typescript
<script setup lang="ts">
</script>
<template>
   <v-responsive class="border rounded" max-height="300">
    <v-app>
      <v-app-bar title="">
        <v-app-bar-title>Top Navigation</v-app-bar-title>
      </v-app-bar>

      <v-navigation-drawer>
        <v-list>
          <v-list-item title="Navigation drawer"></v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-main>
        <v-container>
          <h1>Main Content</h1>
          <v-btn>
            test button
          </v-btn>
        </v-container>
      </v-main>
    </v-app>
  </v-responsive>
</template>
<style scoped>
</style>

```

Reference : 

- [https://vuetifyjs.com/en/getting-started/installation/#existing-projects](https://vuetifyjs.com/en/getting-started/installation/#existing-projects)
- [https://vuetifyjs.com/en/features/icon-fonts/](https://vuetifyjs.com/en/features/icon-fonts/)