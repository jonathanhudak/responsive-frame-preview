<script>
  import { beforeUpdate } from "svelte";
  function getStoredState() {
    try {
      return (
        JSON.parse(localStorage.getItem("state")) || {
          sizes: [[320, 640]]
        }
      );
    } catch {}
  }
  export let state = getStoredState();

  let { sizes, url } = state;

  function addNewSize() {
    sizes = [...sizes, [320, 600]];
  }

  function removeSize(index) {
    let nextSizes = [...sizes];
    nextSizes.splice(index, 1);
    sizes = nextSizes;
  }

  beforeUpdate(() => {
    localStorage.setItem(
      "state",
      JSON.stringify({
        sizes,
        url
      })
    );
  });
</script>

<style>
  :global(body) {
    margin: 0;
    padding: 2rem;
    background: whitesmoke;
  }

  h1 {
  }

  .frames {
    display: flex;
  }

  .frame-container {
    margin-top: 2rem;
    margin-bottom: 2rem;
  }

  .frame {
    border: 1px solid black;
    margin-right: 4rem;
    position: relative;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <h1>Preview a url in multiple viewport sizes</h1>
  <input bind:value={url} placeholder="http://localhost:3000" />

  <div class="frames">
    {#each sizes as [width, height], i}
      <div class="frame-container">
        {width} × {height}
        <div class="frame">
          <iframe
            sandbox="allow-scripts"
            title={url}
            frameborder="0"
            src={url}
            {width}
            {height} />
        </div>
      </div>
    {/each}
  </div>

  {#each sizes as [width, height], i}
    <div>
      <strong>Size {i + 1}</strong>
      <br />
      width:
      <input bind:value={width} type="number" />
      <br />
      height:
      <input bind:value={height} type="number" />
      <button on:click={() => removeSize(i)}>remove size</button>
    </div>
  {/each}
  <button on:click={addNewSize}>Add new size</button>
</main>
