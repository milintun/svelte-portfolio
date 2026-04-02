<script>
    import { base } from '$app/paths';
    export let data = {};
</script>

<article>
    <h2>
        {#if data.slug}
            <a href="{base}/projects/{data.slug}">{data.title}</a>
        {:else}
            {data.title}
        {/if}
    </h2>
    <img src="{base}{data.image}" alt="">
    <p class="year">{data.year}</p>
    <p>{data.description}</p>
</article>

<style>
  article {
    display: grid;
    grid-template-rows: subgrid;
    grid-row: span 4;
  }

  h2 {
    margin: 0;
  }

  a {
    text-align: center;
    transition: all 0.4s;
  }

  a:hover {
    background-color: oklch(from var(--color-accent) 95% 5% h);
    padding: 0.2rem;
  }

  .year {
    font-weight: bold;
    margin-bottom: 0;
  }
</style>