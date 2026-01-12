<script>
  import { CustomMark, usePlot } from 'svelteplot';
  const plot = usePlot();
</script>

<CustomMark>
  {#snippet marks()}
    {@const xScale = plot.scales.x.fn}
    {@const yScale = plot.scales.y.fn}
    
    {@const dx = (v) => Math.abs(xScale(v) - xScale(0))}
    {@const dy = (v) => Math.abs(yScale(v) - yScale(0))}

    <g fill="none" stroke="#555" stroke-width="1.5">
      <!-- Baseline -->
      <line x1={xScale(-250)} y1={yScale(-50)} x2={xScale(250)} y2={yScale(-50)} />

      <!-- Sidelines (Half Court) -->
      <line x1={xScale(-250)} y1={yScale(-50)} x2={xScale(-250)} y2={yScale(420)} />
      <line x1={xScale(250)} y1={yScale(-50)} x2={xScale(250)} y2={yScale(420)} />
      
      <!-- Midcourt Line -->
      <line x1={xScale(-250)} y1={yScale(420)} x2={xScale(250)} y2={yScale(420)} />

      <!-- Three-Point Line -->
      <path d="M {xScale(-220)} {yScale(-50)} 
               V {yScale(89.47)} 
               A {dx(237.5)} {dy(237.5)} 0 0 1 {xScale(220)} {yScale(89.47)} 
               V {yScale(-50)}" />

      <!-- The Paint (16ft wide, extends to y=140 from baseline, so y=190 total) -->
      <!-- In Whitehead coords, the FT line is usually at y=140 -->
      <rect x={xScale(-80)} y={yScale(140)} width={dx(160)} height={dy(190)} />

      <!-- Restricted Area (4ft radius from hoop center) -->
      <path d="M {xScale(-40)} {yScale(0)} 
               A {dx(40)} {dy(40)} 0 0 1 {xScale(40)} {yScale(0)}" />

      <!-- Backboard & Rim -->
      <line x1={xScale(-30)} y1={yScale(-10)} x2={xScale(30)} y2={yScale(-10)} />
      <circle cx={xScale(0)} cy={yScale(0)} r={dx(7.5)} />
      
      <!-- Free Throw Circle -->
      <circle cx={xScale(0)} cy={yScale(140)} r={dx(60)} />
    </g>
  {/snippet}
</CustomMark>