<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import BarHorizontal from '$lib/BarHorizontal.svelte';

  let locData = [];
  let barData = [];
  let commits = [];

  let width = 1000, height = 600;
  let margin = { top: 20, right: 20, bottom: 30, left: 50 };
  let usableArea = {
    top: margin.top,
    right: width - margin.right,
    bottom: height - margin.bottom,
    left: margin.left
  };
  usableArea.width = usableArea.right - usableArea.left;
  usableArea.height = usableArea.bottom - usableArea.top;

  let xAxis, yAxis;

  const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

  function timeOfDay(hour) {
    if (hour >= 5 && hour < 12) return 'Morning';
    if (hour >= 12 && hour < 17) return 'Afternoon';
    if (hour >= 17 && hour < 21) return 'Evening';
    return 'Night';
  }

  $: longestFile = d3.greatest(
    d3.rollups(locData, v => v.length, d => d.file),
    ([, n]) => n
  )?.[0] ?? '';

  $: busiestTimeOfDay = d3.greatest(
    d3.rollups(commits, v => v.length, d => timeOfDay(d.datetime.getHours())),
    ([, n]) => n
  )?.[0] ?? '';

  $: busiestDay = (() => {
    const max = d3.greatest(
      d3.rollups(commits, v => v.length, d => d.datetime.getDay()),
      ([, n]) => n
    );
    return max ? days[max[0]] : '';
  })();

  $: [minDate, maxDate] = d3.extent(commits.map(d => d.date));
  $: maxDatePlusOne = new Date(maxDate);
  $: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);

  $: xScale = d3.scaleTime()
    .domain([minDate, maxDatePlusOne])
    .range([usableArea.left, usableArea.right])
    .nice();

  $: yScale = d3.scaleLinear()
    .domain([24, 0])
    .range([usableArea.bottom, usableArea.top]);

  $: {
    d3.select(xAxis).call(d3.axisBottom(xScale).tickFormat(d3.timeFormat("%b %d, %Y")));
    d3.select(yAxis).call(d3.axisLeft(yScale).tickFormat(d => String(d % 24).padStart(2, "0") + ":00"));
  }

  onMount(async () => {
    locData = await d3.csv(`${base}/loc.csv`, row => ({
      ...row,
      line: Number(row.line),
      depth: Number(row.depth),
      length: Number(row.length),
      date: new Date(row.date + "T00:00" + row.timezone),
      datetime: new Date(row.datetime)
    }));
    barData = d3.rollups(locData, v => v.length, d => d.type)
      .map(([lang, count]) => ({ label: lang, value: count }));
    commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
      let first = lines[0];
      let {author, date, time, timezone, datetime} = first;
      let ret = {
        id: commit,
        url: "https://github.com/hiromitsdm/psychic-octo-invention/commit/" + commit,
        author, date, time, timezone, datetime,
        hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
        totalLines: lines.length,
        lines: lines
      };
      return ret;
    });
  });
</script>

<svelte:head>
  <title>Meta</title>
</svelte:head>

<h1>Meta</h1>

<dl class="stats">
  <div>
    <dt>Total <abbr title="Lines of code">LOC</abbr></dt>
    <dd>{locData.length}</dd>
  </div>
  <div>
    <dt>Total Commits</dt>
    <dd>{commits.length}</dd>
  </div>
  <div>
    <dt>Longest File</dt>
    <dd>{longestFile}</dd>
  </div>
  <div>
    <dt>Busiest Time of Day</dt>
    <dd>{busiestTimeOfDay}</dd>
  </div>
  <div>
    <dt>Busiest Day of Week</dt>
    <dd>{busiestDay}</dd>
  </div>
</dl>

<h3>Commits by time of day</h3>
<svg viewBox="0 0 {width} {height}">
  <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />
  <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />
  <g class="dots">
    {#each commits as commit, index}
      <circle
        cx={xScale(commit.datetime)}
        cy={yScale(commit.hourFrac)}
        r="5"
        fill="steelblue"
      />
    {/each}
  </g>
</svg>

<BarHorizontal data={barData} />

<style>
  svg {
    overflow: visible;
  }

  .stats {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5em 2.5em;
    margin-block: 1.5em;
  }

  .stats > div {
    display: flex;
    flex-direction: column;
  }

  .stats dt {
    font-size: 0.75em;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    opacity: 0.6;
  }

  .stats dd {
    margin: 0;
    font-size: 2em;
    font-weight: bold;
  }
</style>
