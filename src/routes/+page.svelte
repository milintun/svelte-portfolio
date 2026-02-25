<script>
    import { base } from '$app/paths';
    import projects from "$lib/projects";
    import Project from "../lib/Project.svelte";
    import reading from "$lib/reading";
    import ReadingItem from "../lib/ReadingItem.svelte";
</script>
<nav>
    <a class="current" href=".">Home</a>
    <a href="projects">Projects</a>
    <a href="contact">Contact</a>
    <a href="resume">Resume</a>
    <a href="https://github.com/milintun" target="_blank">Github</a>
</nav>
<h1>Milin Tunsiricharoengul</h1>
<p>Hi! I'm Milin. I like drumming for my band and going to concerts with my friends. Welcome to my website!</p>
<img class="milin-pic" src="{base}/images/milin_pic.jpeg" alt="Milin and a friend working on a design project.">

<h3>Latest Projects</h3>
<div class="project-highlights">
    {#each projects.slice(0, 3) as p}
        <Project data={p} />
    {/each}
</div>

<h3>What Milin's Reading</h3>
<div class="reading-list">
    {#each reading as r}
        <ReadingItem data={r} />
    {/each}
</div>

<style>
  .reading-list {
    display: flex;
    flex-direction: row;
    overflow-x: auto;
    gap: 1.5rem;
    padding-bottom: 1rem;
  }
</style>

