# 💚 Vue.js Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Vue 3 (Composition API) quick reference.

---

## Setup

```bash
npm create vue@latest my-app
cd my-app && npm install && npm run dev
```

## Single-File Component (SFC)

```vue
<script setup>
import { ref, computed } from 'vue'

const count = ref(0)
const name = ref('Alan')
const doubled = computed(() => count.value * 2)

function increment() {
  count.value++          // .value in JS; auto-unwrapped in template
}
</script>

<template>
  <h1>Hello {{ name }}</h1>
  <button @click="increment">Count: {{ count }} (x2 = {{ doubled }})</button>
</template>

<style scoped>
h1 { color: teal; }
</style>
```

## Reactivity

```javascript
import { ref, reactive, computed, watch } from 'vue'

const count = ref(0)              // primitive -> use .value
count.value++

const state = reactive({          // object -> deep reactive
  user: 'Alan',
  items: [],
})
state.user = 'Bob'

const total = computed(() => state.items.length)

watch(count, (newVal, oldVal) => {
  console.log(`changed from ${oldVal} to ${newVal}`)
})
```

## Template Syntax

```vue
<template>
  <!-- Interpolation -->
  <p>{{ message }}</p>

  <!-- Attribute binding (v-bind / :) -->
  <img :src="imageUrl" :alt="title">

  <!-- Events (v-on / @) -->
  <button @click="handleClick">Click</button>
  <input @keyup.enter="submit">

  <!-- Two-way binding -->
  <input v-model="name">

  <!-- Class & style -->
  <div :class="{ active: isActive }" :style="{ color: textColor }"></div>
</template>
```

## Conditional & List Rendering

```vue
<template>
  <p v-if="isLoggedIn">Welcome</p>
  <p v-else-if="isGuest">Hi guest</p>
  <p v-else>Please log in</p>

  <span v-show="visible">Toggles display:none</span>

  <ul>
    <li v-for="(item, i) in items" :key="item.id">
      {{ i }}: {{ item.name }}
    </li>
  </ul>
</template>
```

## Props & Emits

```vue
<!-- Child.vue -->
<script setup>
const props = defineProps({
  title: String,
  count: { type: Number, default: 0 },
})
const emit = defineEmits(['save'])

function save() { emit('save', 'payload') }
</script>
```

```vue
<!-- Parent.vue -->
<Child :title="pageTitle" @save="onSave" />
```

## Lifecycle Hooks

```javascript
import { onMounted, onUnmounted, onUpdated } from 'vue'

onMounted(() => {
  // fetch data, access DOM
  fetch('/api/posts').then(r => r.json()).then(d => posts.value = d)
})
onUnmounted(() => { /* cleanup */ })
```

## v-model on Components

```vue
<!-- Custom input -->
<script setup>
const model = defineModel()   // Vue 3.4+
</script>
<template><input v-model="model" /></template>
```

## Composables (reusable logic)

```javascript
// useCounter.js
import { ref } from 'vue'
export function useCounter(start = 0) {
  const count = ref(start)
  const increment = () => count.value++
  return { count, increment }
}

// in component
const { count, increment } = useCounter()
```

## Vue Router & Pinia (ecosystem)

```javascript
// router
const routes = [
  { path: '/', component: Home },
  { path: '/user/:id', component: User },
]

// Pinia store
import { defineStore } from 'pinia'
export const useStore = defineStore('main', {
  state: () => ({ count: 0 }),
  actions: { inc() { this.count++ } },
})
```

---

[🔝 Back to README](../README.md)
