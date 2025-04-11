<script>
  import Chart from "./Chart.svelte";

  let { countryData, minWidth=260 } = $props();
  const margin = {top: 20, right: 20, bottom: 20, left: 20}
  let width = $state(260);
  let innerWidth = $derived(width - margin.left - margin.right);
  let height = $derived(width);
  
</script>

<div class="chart-container" 
  bind:clientWidth={width}>
  <p class="country-header">{countryData[0].country}</p>  
  <svg width={width} height={height}>
    <Chart {countryData} width={width} />
  </svg>
</div>

<style>
  .country-header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 60px;
    border-bottom: 1px solid;
    background-color: var(--card-header-background-color);
    border-radius: 16px 16px 0 0;
    font-size: 1.8rem;
    font-weight: bold;
  }

  .chart-container  {
    /* border: 1px solid gray; */
    border-radius: 16px;
    display: flex;
    flex-direction: column;
    gap: 0px;
    box-shadow: rgba(60, 64, 67, 0.3) 0px 1px 2px 0px, rgba(60, 64, 67, 0.15) 0px 2px 6px 2px;
  }  
</style>