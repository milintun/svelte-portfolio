<script>
    import { base } from '$app/paths';
    export let data = {}
</script>

<div class="book">
    <img class="book-img" src="{base}/{data.image}" alt="{data.title}">
    <h3>{data.title}</h3>
    <p>{data.author}</p>
</div>

<style>
  .book {
    display: flex;
    flex-direction: column;
    align-items: start;
    flex-shrink: 0;
    width: 8rem;
  }

  .book-img {
    width: 8rem;
    height: 11rem;
    object-fit: cover;
  }

  h3 {
    margin: 0.5rem 0 0rem;
    font-size: 0.8rem;
    line-height: 1.3;
  }

  p {
    margin: 0;
    font-size: 0.6rem;
    color: oklch(55% 3% 200);
  }
</style>