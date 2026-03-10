<script>
    import { base } from '$app/paths';
    import projects from "$lib/projects";
    import Project from "../lib/Project.svelte";
    import reading from "$lib/reading";
    import ReadingItem from "../lib/ReadingItem.svelte";
    import { onMount } from "svelte";

    let githubData = null; // This will eventually hold our Github stats
    let loading = true; // This will be true *until* the fetch's promise resolves to a value
    let error = null; // If the API call resulted in an error, it will go into this variable

    onMount(async () => {
        try {
            console.log("Page has been mounted!")
            let response = await fetch("https://api.github.com/users/milintun");
            console.log(response);
            githubData = await response.json();
            console.log(githubData);
        } catch (err) {
            error = err;
        }
        loading = false;
    })
</script>

<h1>Milin Tunsiricharoengul</h1>
<p>Hi! I'm Milin. I like drumming for my band and going to concerts with my friends. Welcome to my website!</p>
<img class="milin-pic" src="{base}/images/milin_pic.jpeg" alt="Milin and a friend working on a design project.">

{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>Something went wrong: {error.message}</p>
{:else}
    <section class="github-stats">
        <h2>My GitHub Stats</h2>
        <dl class="stats-grid">
            <div class="stat-card">
                <dt>Followers</dt>
                <dd>{githubData.followers}</dd>
            </div>
            <div class="stat-card">
                <dt>Following</dt>
                <dd>{githubData.following}</dd>
            </div>
            <div class="stat-card">
                <dt>Public Repos</dt>
                <dd>{githubData.public_repos}</dd>
            </div>
        </dl>
    </section>
{/if}

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

  .github-stats {
    margin: 1.5rem 0;
  }

  .stats-grid {
    display: flex;
    gap: 1rem;
    margin: 0;
    padding: 0;
    flex-wrap: wrap;
  }

  .stat-card {
    background: linear-gradient(135deg, #1a1a2e, #16213e);
    border: 2px solid #0f3460;
    border-radius: 12px;
    padding: 1.2rem 1.5rem;
    text-align: center;
    flex: 1;
    min-width: 120px;
    box-shadow: 0 4px 15px rgba(15, 52, 96, 0.4);
    transition: transform 0.2s, box-shadow 0.2s;
  }

  .stat-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 25px rgba(15, 52, 96, 0.6);
  }

  .stat-card dt {
    font-size: 0.75rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: #e94560;
    margin-bottom: 0.4rem;
  }

  .stat-card dd {
    font-size: 2.2rem;
    font-weight: 800;
    color: #eaeaea;
    margin: 0;
    line-height: 1;
  }
</style>

