<script>
    export let data;
    export let colorScale;
    export let width;
    // console.log(data);
    import {fly, fade} from 'svelte/transition';
    let tooltipWidth;

  const xNudge = 5;
  const yNudge = 5; 

  // If the x position + the tooltip width exceeds the chart width, offset backward to prevent overflow
  $: xPosition =
    data.x + tooltipWidth + xNudge > width
      ? data.x - tooltipWidth - xNudge
      : data.x + xNudge;
  $: yPosition = data.y + yNudge;
   </script>
  
  <div class='tooltip'
  in:fly={{y: 10, duration: 200,delay: 200}}
    style="position: absolute;
    top: {yPosition}px;
    left: {xPosition}px;"
   bind:clientWidth={tooltipWidth}
  >
    <!-- Country name -->
    <h1>
        {data.country}
      </h1>
   <!-- Additional info under the country name -->
<div class='info'>
    <span class="score">
        {data.happiness}/10
    </span>
    <span class="continent" style="background:{colorScale(data.continent)}">
        {data.continent}
    </span> 
    <span class="bar background" />
    <span class="bar foreground" 
    style="width: {data.happiness * 10}%; 
     background: {colorScale(data.continent)}" />
</div>
  </div>

<style>
    .tooltip {
        background-color: white;
        border: 1px solid black;
        padding: 0.5rem;
        border-radius: 0.25rem;
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        transition: top 300ms ease, left 300ms ease;
    }
    .info {
    display: flex;
    justify-content: space-between;
    column-gap: 8px;
}
.score {
    font-size: 0.8rem;
}
.continent {
    font-size: 0.65rem;
    padding: 3px 4px 2px 4px;
    border-radius: 3px;
    text-transform: uppercase;
    white-space: nowrap; /* Prevents line breaks */
}
.bar {
    position: absolute;
    bottom: 0; /* Forces to bottom of tooltip */
    left: 0; /* Forces to left of tooltip */
    height: 3px; 
    width: 100%;
}
.bar.background {
    background: #eee;
}
</style>