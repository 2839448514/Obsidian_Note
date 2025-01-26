## main.js
```js
import './assets/main.css'
import { createApp } from 'vue'
import App from './App.vue'
createApp(App).mount('#app')
```
## App.vue
```vue
<template>
  <div class="app">
    {{ message }}
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      message: 'Hello World!',
    }
  },
  methods: {
    fun() {
      alert('Hello World!')
    },
  },
}
</script>

<style>
.app {
  color: red;
}
</style>

```