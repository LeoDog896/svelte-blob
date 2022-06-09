<script lang="ts">
  import Blob from "$lib/Blob.svelte"

  let pointAmount = 6
  let debugCircles = false
  let debugCircleRadius = 3
  let deformation = 0.1
  let key = {}
  let path = ""
</script>

<div class="m-8">
  <div class="p-8 rounded-lg border border-gray-300 inline-block">
    {#key key}
      <Blob {pointAmount} {debugCircles} bind:path={path} {debugCircleRadius} {deformation}></Blob>
    {/key}
  </div>
  <p>Path: {path}</p>
  <div class="block">
    <label for="pointAmount">Amount of points: </label>
    <input id="pointAmount" type="range" min=3 step=1 max=40 bind:value={pointAmount}>
    {pointAmount}
  </div>
  <div class="block">
    <label for="deformation">Deformation: </label>
    <input id="deformation" type="range" min=0 max=1 step=0.01 bind:value={deformation}>
    {deformation}
  </div>
  <div class="block">
    <label for="debugCircles">Debug circles: </label>
    <input id="debugCircles" type="checkbox" bind:checked={debugCircles}>
    {debugCircles}
  </div>
  <div class="block">
    <label for="debugCircleRadius">Debug circle radius: </label>
    <input id="debugCircleRadius" disabled={!debugCircles} type="range" min=1 max=10 bind:value={debugCircleRadius}>
    {debugCircleRadius}
  </div>
  <div class="block">
    <button on:click={() => key = {}}>Randomize</button>
  </div>
</div>