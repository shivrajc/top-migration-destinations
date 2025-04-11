<script>
  let label = "yes";

  let { data, index } = $props();

  let hiddenArc;
  let calculatePath =(d) =>{    
    const firstArcSection = /(^.+?)L/;
    hiddenArc = firstArcSection.exec(d.path)[1];

    hiddenArc = hiddenArc.replace(/,/g, " ");
    if (d.index === 2 || d.index === 3) {

      let startLoc 	= /M(.*?)A/,		
        middleLoc 	= /A(.*?)0 0 1/,	
        endLoc 		= /0 0 1 (.*?)$/;	

      let newStart = endLoc.exec(hiddenArc)[1];
      let newEnd = startLoc.exec(hiddenArc)[1];
      let middleSec = middleLoc.exec(hiddenArc)[1];        
      
      hiddenArc = "M" + newStart + "A" + middleSec + "0 0 0 " + newEnd;
    }  
        
    return  hiddenArc;
  }

</script>

<path        
  class="hiddenArcs"
  id={label === "yes" ? `hiddenArc-${data.data.criteria}-${index}` : `hiddenArc-${data.data.score}-${index}`}
  d={calculatePath(data)}
  fill="none"   
/>

<text
  class="donutText"
  class:chart-labels={label === "yes"}
  dy={index === 2 || index === 3 ? "-11" : "20"}      
  fill="black"  
  >
  <textPath
  startOffset="50%"
  text-anchor="middle"
  xlink:href={label === "yes" ? `#hiddenArc-${data.data.criteria}-${index}` : `#hiddenArc-${data.data.score}-${index}`}
  >
  {data.data.criteria}
</textPath>
</text>

<style>
  path {
    pointer-events: none;
  }
  
  .donutText {
    font-size: 14px;    
    font-weight: 500;    
    pointer-events: none;
  }

  .chart-labels {
      font-size: 11px;
      text-transform: uppercase;
      letter-spacing: 1px;      
      pointer-events: none;      
  }
</style>