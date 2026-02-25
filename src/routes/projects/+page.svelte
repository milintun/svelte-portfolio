<script>
  import projects from "$lib/projects.json"
  import Project from "../../lib/Project.svelte";
</script>

<nav>
  <a href="./">Home</a>
  <a class="current" href="projects">Projects</a>
  <a href="contact">Contact</a>
  <a href="resume">Resume</a>
  <a href="https://github.com/milintun" target="_blank">Github</a>
</nav>

<h1>{ projects.length } Projects</h1>
<div class="projects">
    {#each projects as p}
    <Project data={p} />
    {/each}
</div>
