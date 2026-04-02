<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import BarHorizontal from '$lib/BarHorizontal.svelte';

  let locData = [];
  let barData = [];
  let commits = [];

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
    console.log(commits);
  });
</script>

<svelte:head>
  <title>Meta</title>
</svelte:head>

<h1>Meta</h1>
<p>Meta page to visualize project data.</p>

<BarHorizontal data={barData} />
