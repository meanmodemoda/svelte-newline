<script>
    export let data;
    export let xScale;
    export let yScale;
    export let width;

    let tooltipWidth;
    const xNudge=10;
    const yNudge=20;
    $: x= xScale(data.grade);
    $: y= yScale(data.hours);
    $: isFallingOffChart=tooltipWidth+x>width
    $: xPosition = isFallingOffChart ? x-tooltipWidth-xNudge : x+xNudge;
    $: yPosition = y+yNudge;
    $: console.log(x, y)
    $: console.log(tooltipWidth+x>width)

    import {fly} from 'svelte/transition';
 
</script>

<div in:fly={{y:-20, duration: 300}}
out:fly={{y:20, duration: 300}}
bind:clientWidth={tooltipWidth}
class="tooltip"
style="left:{xPosition}px; top:{yPosition}px">
    <h1>{data.name}<span>{data.grade}%</span></h1>
    <h2>{data.hours}hours studied </h2>
</div>
  <style>
    .tooltip {
      position: absolute;
      padding: 8px 6px;
    background: white;
    box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
    border-radius: 3px;
    pointer-events: none;
    transition: top 300ms ease, left 300ms ease;
    }

    h1 {
    font-size: 1rem;
    font-weight: 500;
    margin-bottom: 6px;
    width: 100%;
}

h2 {
    font-size: 0.8rem;
    font-weight: 300;
    text-transform: uppercase;
}
span {
    background: #dda0dd50;
    font-size: 80%;
    margin-left: 2px;
    padding: 2px 4px;
    display: inline-block;
    vertical-align: bottom;
    border-radius: 3px;
    color: rgba(0, 0, 0, 0.7);
}
  </style>