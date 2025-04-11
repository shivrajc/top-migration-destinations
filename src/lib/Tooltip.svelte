<script>
  import { scaleLinear } from "d3";
  let { criteriaData, selectedCountry } = $props(); 

  const width = 300;
  const margin = {top: 20, right: 20, bottom: 20, left: 20}
  const innerWidth = width - margin.left - margin.right;
  const xScale = scaleLinear()
    .domain([0, 100])
    .range([0, innerWidth])
</script>
<!-- 
<div class="container">
  <p class="heading">{criteriaData[0].criteria}</p>
  <div class="chart">    
  {#each criteriaData.sort((a, b) => b.score - a.score) as d, i}
    <div class="country-row">
      <span class="country-name">{i+1}. {d.country}</span>
      <span 
        class="country-score"
        style="width: {xScale(d.score)}px;"
        class:hovered={d.country === selectedCountry}        
      >
        {d.score}
      </span>
    </div>
  {/each}
  </div>
</div> -->

<div class="container">
  <p class="heading">{criteriaData[0].criteria}</p>
  <div class="metrics">
    <div class="metric-wrapper">
      <span class="label">Criteria: </span>
      <span class="value">{criteriaData[0].criteria}</span>
    </div>
    <div class="metric-wrapper">
      <span class="label">Country: </span>
      <span class="value">{selectedCountry}</span>
    </div>
    <div class="metric-wrapper">
      <span class="label">Rank: </span>
      <span class="value">{criteriaData.sort((a, b) => b.score - a.score).map(c => c.country).indexOf(selectedCountry)+1}</span>
    </div>
    <div class="metric-wrapper">
      <span class="label">Score: </span>
      <span class="value">{criteriaData.filter(d => d.country === selectedCountry)[0].score}</span>
    </div>
  </div>

</div>

<style>
  /* .chart {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: repeat(auto, 1fr);    
    gap: 4px;
  } */

  .heading {
    font-size: 1.2rem;
    font-weight: bold;
    margin-bottom: 8px;
  }

  .label {
    color: gray;
  }

  .value {
    font-weight: 500;
  }

  /* .country-row {
    display: flex;
    gap: 20px;
    justify-content: flex-start;
    align-items: center;
    font-size: 14px;
    
  }

  .country-name {
    width: 120px;
  }

  .country-score {
    background-color: rgb(197, 197, 197);    
    padding: 0px 12px;
    border-radius: 12px;
  }

  .hovered {
    background-color: cornflowerblue;
    color: white;
  } */
</style>