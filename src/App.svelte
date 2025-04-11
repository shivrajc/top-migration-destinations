<script>
  import Charts from './lib/Charts.svelte';
  // import viteLogo from '/vite.svg'
  // import Counter from './lib/Counter.svelte'
  import { data } from './lib/data';
  import Footer from './lib/Footer.svelte';
  import Header from './lib/Header.svelte';
  import { hoveredData } from './lib/shared.svelte';
  import Tooltip from './lib/Tooltip.svelte';

  const minWidth = 320;
  const countries = [...new Set($data.map(d => d.country))];

  let criteriaData = $derived($data.filter(d => d.criteria === hoveredData.criteria));
  let selectedCountry = $derived(hoveredData.country);

</script>

<main>
  <div class="header">
    <Header />
  </div>
  <hr>
  <div class="chart-container" style="grid-template-columns: repeat(auto-fit, minmax({minWidth}px, 1fr));">
    {#each countries as country}
      <Charts countryData={$data.filter(d => d.country === country)} {minWidth} />
    {/each}
  </div>

  {#if hoveredData.criteria !== ""}
    <div 
      class="tooltip"
      style="top: {hoveredData.y}px; left: {hoveredData.x}px;"
    
    >    
      <Tooltip {criteriaData} {selectedCountry} /> 
    </div>    
  {/if}
</main>
<Footer />

<style>
  .chart-container {
    display: grid;    
    gap: 40px;
  }

  main {
    position: relative;
    margin-bottom: 24px;
  }

  .tooltip {
    position: absolute;    
    background-color: white;
    padding: 12px;
    border: 1px solid gray;
    border-radius: 8px;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
  }

  hr {
    margin: 40px auto;
    width: 40px;
    height: 2px;
    background-color: black;
    border: none;
    border-radius: 12px;
  }
</style>
