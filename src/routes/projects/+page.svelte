<script>
  import projects from "$lib/projects.json"
  import Project from "../../lib/Project.svelte";
  import ProjectNarrative from "../../lib/ProjectNarrative.svelte";
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Bar from "../../lib/Bar.svelte";

  let rawData = [];

  let wrangled = [];
  let percentages = [];

  onMount(async () => {
      rawData = await d3.json('/lab6_example.json');
      wrangled = d3.rollups(
          rawData,
          v => d3.sum(v, d => d.lines),
          d => d.language
      );
      const totalLines = d3.sum(wrangled.map((entry) => entry[1]))
      percentages = wrangled.map(([language, lines]) => {
        return [language, lines / totalLines * 100]
      })
      
  });

  let years = projects.map(proj => proj.year)
  let range = Math.max(...years) - Math.min(...years);

  $: barData = d3.rollups(projects, v => v.length, d => d.year)
  .map(([year, count]) => ({ label: String(year), value: count }));

</script>

<section>
    <h2>Data wrangling result</h2>
    <pre>{JSON.stringify(wrangled, null, 2)}</pre>

    <h2>Data wrangling percentages exercise</h2>
    <pre>{JSON.stringify(percentages, null, 2)}</pre>
    <Bar data={barData}/>
</section>

<h1>{ projects.length } Projects over {range} Years</h1>
<p>Scroll down to see my a timeline of my projects and how they've contributed to my professional and personal life</p>
<ProjectNarrative />
<p class="outro">Thanks for scrolling through my project story! Feel free to explore all of the projects at your leisure below.</p>
<div class="projects">
    {#each projects as p}
    <Project data={p} />
    {/each}
</div>


<style>
  .outro {
    margin-bottom: 2rem;
  }

  .projects {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
    grid-template-rows: auto;
    gap: 1rem;
  }
</style>