<script>
  import * as d3 from 'd3';

  export let data = [];

  let width = 1000, height = 300;
  let margin = { top: 20, right: 20, bottom: 30, left: 50 };
  let usableArea = {
    top: margin.top,
    right: width - margin.right,
    bottom: height - margin.bottom,
    left: margin.left
  };

  let xAxis, yAxis;

  $: xScale = d3.scaleTime()
    .domain(d3.extent(data, d => d.date))
    .range([usableArea.left, usableArea.right]);

  $: yScale = d3.scaleLinear()
    .domain([0, d3.max(data, d => d.count)])
    .range([usableArea.bottom, usableArea.top])
    .nice();

  $: line = d3.line()
    .x(d => xScale(d.date))
    .y(d => yScale(d.count));

  $: if (xAxis && yAxis) {
    d3.select(xAxis).call(d3.axisBottom(xScale));
    d3.select(yAxis).call(d3.axisLeft(yScale));
  }
</script>

<h3>Lines Edited by Day</h3>
<svg viewBox="0 0 {width} {height}">
  <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
  <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
  <path
    d={line(data)}
    fill="none"
    stroke="steelblue"
    stroke-width="2"
  />
</svg>

<style>
  svg {
    overflow: visible;
    max-width: 100%;
    height: auto;
  }
</style>
