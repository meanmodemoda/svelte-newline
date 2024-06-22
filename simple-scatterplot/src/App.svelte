<script>
  import AxisX from "$components/AxisX.svelte";
  import AxisY from "$components/AxisY.svelte";
  import Tooltip from "$components/Tooltip.svelte";
  import data from "$data/data.js";

  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";

  let width = 400;
  $: console.log(width)
  let height = 400;

  const margin = { top: 20, right: 15, bottom: 20, left: 0 };
  $: innerWidth = width - margin.right - margin.left;
  $: xScale = scaleLinear()
    .domain([0, 100])
    .range([0, innerWidth]);
  const innerHeight = height - margin.top - margin.bottom;

  let yScale = scaleLinear()
    .domain([0, max(data, d => d.hours)])
    .range([innerHeight, 0]);
  let hoveredData;
$: console.log(hoveredData)
 
 
</script>
<div class="chart-container" bind:clientWidth={width}>

<svg {width} {height}
on:mouseleave={() =>hoveredData=null}>
  <g class='inner-chart' transform="translate({margin.left}, {margin.top})">
    <AxisY width={innerWidth} {yScale} />
    <AxisX height={innerHeight} width={innerWidth} {xScale} />
    <!-- sorting data for tab -->
    {#each data.sort((a,b)=>a.grade-b.grade) as d}
      <circle
        cx={xScale(d.grade)}
        cy={yScale(d.hours)}
        r={hoveredData === d ? 20:10}
        opacity={hoveredData? (hoveredData === d ? 1:0.65) : 1}
        fill="purple"
        stroke="black"
        stroke-width={1}
        on:mouseover={() =>hoveredData=d}
        on:focus={() =>hoveredData=d}
     tabindex="0"

      
      />
    {/each}
  </g>
</svg>
{#if hoveredData}
<Tooltip data={hoveredData} {xScale} {yScale} width={innerWidth} />
{/if}
</div>

<style>
  :global(.tick text, .axis-title) {
    font-weight: 400; /* How thick our text is */
    font-size: 12px; /* How big our text is */
    fill: hsla(212, 10%, 53%, 1); /* The color of our text */
  }
  circle {
    transition: r 300ms, opacity 500ms ease;
  }
</style>