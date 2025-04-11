<script>
  import { scaleBand, scaleRadial, scaleLinear, scaleSequential, interpolateViridis, arc, extent } from "d3";
  import Gridlines from "./Gridlines.svelte";
  import DonutChart from "./DonutChart.svelte";
  import { scale } from "svelte/transition";
  import { hoveredData } from "./shared.svelte";

  let { countryData, width } = $props();  

  const criteriaData = countryData.map(d => ({
    criteria: d.criteria, value: 1
  }));

  const margin = {top: 40, right: 40, bottom: 40, left: 40};
  let innerWidth = $derived(width - margin.left - margin.right);
  let innerRadius = $derived(width/10);
  let outerRadius = $derived(innerWidth/2);

  let xScale = $derived(scaleBand()
    .domain(countryData.map(d => d.criteria))
    .range([0, 2 * Math.PI]));

  let yScale = $derived(scaleRadial()
    .domain([0, 100])
    .range([innerRadius, outerRadius]));

  const colorScale = $derived(scaleSequential()
    .domain([0, 100])
    .interpolator(interpolateViridis));

  let getCoordinatesForAngle = $derived((angle, offset=1) => ({
    x: Math.cos(angle) * outerRadius * offset,
    y: Math.sin(angle) * outerRadius * offset,
  }))

  let rScale = $derived(scaleLinear()
    .domain([0, 100])
    .range([innerRadius, outerRadius])
    .nice());

  let arcGenerator = $derived(arc()
    .innerRadius(innerRadius)
    .outerRadius(d => rScale(d.score))
    .startAngle(d => xScale(d.criteria))
    .endAngle(d => xScale(d.criteria) + xScale.bandwidth())
    .padAngle(0.01)
    .padRadius(innerRadius));

</script>

<g transform="translate({width/2}, {width/2})">
  {#each countryData.map(d => d.criteria) as c}
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <!-- svelte-ignore a11y_mouse_events_have_key_events -->
    <path 
      d={arcGenerator(countryData.filter(d => d.criteria === c)[0])} 
      fill="{colorScale(countryData.filter(d => d.criteria === c)[0].score)}"
      onmouseover={(e) => {
        hoveredData.criteria = c;
        hoveredData.country = countryData[0].country;
        hoveredData.x = e.layerX;
        hoveredData.y = e.layerY;
      }}
      onmouseout={() => {
        hoveredData.criteria = "";
        hoveredData.country = "";        
      }}
      class:hovered={hoveredData.criteria === c && hoveredData.country === countryData[0].country}
    />

    <line 
      x2={getCoordinatesForAngle(xScale(c) - Math.PI / 2).x} 
      y2={getCoordinatesForAngle(xScale(c) - Math.PI / 2).y} 
      class="grid-lines"/>
  {/each}

  <Gridlines {rScale} />

  <circle
    cx={0}
    cy={0}
    r={innerRadius}
    fill="white"
    class="grid-lines"
    shape-rendering="crispEdges"
  />  

  <DonutChart dataset={criteriaData} {innerRadius} {outerRadius} />
</g>

<g class="total-label">
  <text
    x={width/2}
    y={width/2-4}
    class="total-label-value"
  >
    {countryData[0].total_score}
  </text>
  <text
    x={width/2}
    y={width/2+16}
    class="total-label-text"
  >
    TOTAL
  </text>  
</g>

<style>
  .grid-lines {
    stroke: var(--grid-line-color);
  }

  .total-label-value {
    font-size: 1.6rem;
    font-weight: bold;
  }

  .total-label-text {
    font-size: 0.7rem;    
    
  }
  
  .total-label {
    text-anchor: middle;
    dominant-baseline: middle;    
  }

  .hovered {
    stroke: black;
    stroke-width: 3px;
  }
</style>