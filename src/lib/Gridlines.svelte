<script>
  let { rScale } = $props()
</script>

<g>
  {#each rScale.ticks(4) as tick}
    <circle
      r={rScale(tick)}
      fill="none"
      stroke="black"      
      stroke-width="1"
      shape-rendering="crispEdges"
      class="grid-lines"
    />

    <!-- <rect 
      y={rScale(tick) - 10}
      width="20"
      height="20"
      fill="white"
    /> -->
    <text
      x=2
      y={rScale(tick)-2}
      class="tick-labels"
    >
      {tick}
    </text>    
  {/each}  
</g>


<style>
  .grid-lines {
    stroke: var(--grid-line-color);
    pointer-events: none;
  }
  
  .tick-labels {
    font-size: 0.6rem;
    fill: "lightgray";    
    pointer-events: none;
  }
</style>  
