# 🧡 Svelte Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Svelte quick reference.

---

## Setup

```bash
npm create vite@latest my-app -- --template svelte
# or full-stack: npx sv create my-app   (SvelteKit)
cd my-app && npm install && npm run dev
```

## Component Structure

```svelte
<script>
  let name = 'Alan';
  let count = 0;

  function increment() {
    count += 1;          // plain assignment triggers reactivity
  }
</script>

<h1>Hello {name}</h1>
<button on:click={increment}>Count: {count}</button>

<style>
  h1 { color: tomato; }   /* scoped by default */
</style>
```

## Reactivity

```svelte
<script>
  let count = 0;

  // reactive declaration ($:) — recomputes when deps change
  $: doubled = count * 2;
  $: if (count > 10) console.log('big');

  let items = [];
  // reassign (not .push) to trigger updates
  function add(item) { items = [...items, item]; }
</script>

<p>{count} doubled is {doubled}</p>
```

## Binding

```svelte
<script>
  let name = '';
  let agreed = false;
  let selected = 'a';
</script>

<input bind:value={name} />            <!-- two-way -->
<input type="checkbox" bind:checked={agreed} />
<select bind:value={selected}>
  <option value="a">A</option>
</select>

<!-- bind to DOM element -->
<div bind:this={element}></div>
```

## Logic Blocks

```svelte
{#if isLoggedIn}
  <p>Welcome</p>
{:else if isGuest}
  <p>Hi guest</p>
{:else}
  <p>Please log in</p>
{/if}

{#each items as item, i (item.id)}
  <li>{i}: {item.name}</li>
{:else}
  <p>No items</p>
{/each}

{#await promise}
  <p>Loading...</p>
{:then data}
  <p>{data}</p>
{:catch error}
  <p>{error.message}</p>
{/await}
```

## Events

```svelte
<button on:click={handleClick}>Click</button>
<button on:click={() => count++}>Inline</button>
<input on:keyup={(e) => console.log(e.key)} />

<!-- Modifiers -->
<form on:submit|preventDefault={handleSubmit}>
<button on:click|once={doOnce}>
```

## Props

```svelte
<!-- Child.svelte -->
<script>
  export let title;              // prop
  export let count = 0;          // prop with default
</script>
<h2>{title}: {count}</h2>
```

```svelte
<!-- Parent.svelte -->
<script> import Child from './Child.svelte'; </script>
<Child title="Hi" count={5} />
```

## Component Events (child -> parent)

```svelte
<!-- Child.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();
  function save() { dispatch('save', { id: 1 }); }
</script>

<!-- Parent.svelte -->
<Child on:save={(e) => console.log(e.detail)} />
```

## Lifecycle

```svelte
<script>
  import { onMount, onDestroy } from 'svelte';

  onMount(() => {
    fetch('/api/posts').then(r => r.json()).then(d => posts = d);
    return () => console.log('cleanup');
  });

  onDestroy(() => { /* teardown */ });
</script>
```

## Stores (shared state)

```javascript
// stores.js
import { writable, derived } from 'svelte/store';
export const count = writable(0);
export const doubled = derived(count, $c => $c * 2);
```

```svelte
<script>
  import { count } from './stores.js';
  // $ prefix auto-subscribes
</script>
<button on:click={() => $count++}>{$count}</button>
```

---

[🔝 Back to README](../README.md)
