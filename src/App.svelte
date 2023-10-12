<script>
  // import svelteLogo from './assets/svelte.svg'
  // import viteLogo from '/vite.svg'
  // import Counter from './lib/Counter.svelte'
  import lec3d from '@trickle/lec3d'
  import { onMount } from 'svelte';

  let element;
  onMount(() => {
    console.log('ele', element)
    const { scene, mountTo, addControls} = lec3d.init({
      axesHelperConfigs: {
        length: 1000
      }
    })

    addControls({
      callback: (scene, camera) => {
        console.log(scene, camera)
      }
    })


    lec3d.loadGLTF({ 
      modelPath: 'computers/scene.gltf', 
      options: {
        scale: 50,
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})


    mountTo(element)
  })


</script>

<main>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank" rel="noreferrer">
      <img src={viteLogo} class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank" rel="noreferrer">
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>

  <div class="card">
    <Counter />
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank" rel="noreferrer">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  </p> -->
  <div bind:this={element}></div>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
