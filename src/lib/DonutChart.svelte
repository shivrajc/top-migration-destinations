<script>
  import * as d3 from "d3";
  import HiddenArc from "./HiddenArc.svelte";

  let {dataset, innerRadius, outerRadius} = $props();


  let createPieData = (data) => {
      // const pie = d3.pie().value(1);
      const pie = d3.pie()        
                      .startAngle(-Math.PI * 2)
                      .endAngle(Math.PI)
                      .padAngle(0.02)
                      .value(1);
      const arcGenerator = d3
          .arc()
          .innerRadius(innerRadius)
          .outerRadius(outerRadius + outerRadius * 0.18);

      return pie(data).map((d, i) => {
          return {
              data: d.data,
              index: i,
              path: arcGenerator(d),
              fill: "black",
              centroid: arcGenerator.centroid(d),
              startAngle: d.startAngle,
              endAngle: d.endAngle,
          };
      });
  }       

  let pieData = $derived(createPieData(dataset));



</script>

<g>
  {#each pieData as d, i}    
      <HiddenArc            
        data={d} 
        index={i} 
      />
  {/each}

</g>
