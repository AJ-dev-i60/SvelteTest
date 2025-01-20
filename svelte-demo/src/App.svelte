<script>
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import { fade, fly, slide } from 'svelte/transition';
  
  // Demo features
  const features = [
    { id: 1, title: 'Reactive State', description: 'Automatic UI updates when data changes' },
    { id: 2, title: 'Smooth Animations', description: 'Built-in transition system' },
    { id: 3, title: 'Component Composition', description: 'Easy to build and combine components' },
    { id: 4, title: 'Two-way Binding', description: 'Simplified form handling' },
    { id: 5, title: 'Minimal Bundle', description: 'Tiny runtime and optimal output' }
  ];

  // Reactive state examples
  let count = 0;
  let name = '';
  let selectedFeature = null;
  
  // Store example
  const theme = writable('light');
  
  // Reactive declaration
  $: doubledCount = count * 2;
  
  // Theme toggle
  function toggleTheme() {
    theme.update(t => t === 'light' ? 'dark' : 'light');
  }

  // Animation demo
  let showAnimationDemo = false;
  
  onMount(() => {
    // Auto-cycle through features
    const interval = setInterval(() => {
      count = (count + 1) % 100;
    }, 1000);
    
    return () => clearInterval(interval);
  });
</script>

<main class={$theme}>
  <div class="header" transition:slide>
    <h1>Svelte Demo Showcase</h1>
    <button on:click={toggleTheme}>Toggle Theme</button>
  </div>

  <section class="features">
    {#each features as feature (feature.id)}
      <div
        class="feature-card"
        on:click={() => selectedFeature = feature}
        in:fly="{{ y: 50, duration: 500, delay: feature.id * 100 }}"
        animate:flip
      >
        <h3>{feature.title}</h3>
        <p>{feature.description}</p>
      </div>
    {/each}
  </section>

  <section class="interactive-demo">
    <div class="counter-demo">
      <h2>Reactive State Demo</h2>
      <p>Count: {count}</p>
      <p>Doubled: {doubledCount}</p>
      <button on:click={() => count++}>Increment</button>
    </div>

    <div class="binding-demo">
      <h2>Two-way Binding Demo</h2>
      <input bind:value={name} placeholder="Type your name">
      {#if name}
        <p transition:fade>Hello, {name}!</p>
      {/if}
    </div>

    <div class="animation-demo">
      <h2>Animation Demo</h2>
      <button on:click={() => showAnimationDemo = !showAnimationDemo}>
        Toggle Animation
      </button>
      
      {#if showAnimationDemo}
        <div class="animated-box" transition:fly="{{ y: 200, duration: 1000 }}">
          <p>Smooth animations!</p>
        </div>
      {/if}
    </div>
  </section>
</main>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  main {
    padding: 2rem;
    min-height: 100vh;
    transition: background-color 0.3s, color 0.3s;
  }

  main.light {
    background-color: #ffffff;
    color: #1a1a1a;
  }

  main.dark {
    background-color: #1a1a1a;
    color: #ffffff;
  }

  .header {
    text-align: center;
    margin-bottom: 2rem;
  }

  .features {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
    margin-bottom: 2rem;
  }

  .feature-card {
    background: rgba(255, 255, 255, 0.1);
    padding: 1.5rem;
    border-radius: 8px;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    border: 1px solid rgba(255, 255, 255, 0.1);
  }

  .feature-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
  }

  .interactive-demo {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 2rem;
  }

  button {
    background: #ff3e00;
    color: white;
    border: none;
    padding: 0.5rem 1rem;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
  }

  button:hover {
    background: #ff6240;
  }

  input {
    padding: 0.5rem;
    border-radius: 4px;
    border: 1px solid #ccc;
    width: 100%;
    max-width: 300px;
  }

  .animated-box {
    background: #ff3e00;
    color: white;
    padding: 2rem;
    border-radius: 8px;
    margin-top: 1rem;
    text-align: center;
  }

  h1 {
    font-size: 2.5rem;
    margin-bottom: 1rem;
  }

  h2 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }
</style>
