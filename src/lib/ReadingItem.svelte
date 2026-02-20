<script>
  export let data = {};
</script>

<article>
  <img src={data.image} alt={data.title}>
  <h3>{data.title}</h3>
  <p class="author">{data.author}</p>
</article>

<style>
  article {
    border: 1px solid #ddd;
    padding: 1rem;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
  }

  img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 4px;
    margin-bottom: 1rem;
  }

  h3 {
    margin: 0 0 0.5rem 0;
    font-size: 1rem;
    line-height: 1.3;
  }

  .author {
    margin: 0;
    font-size: 0.9rem;
    color: #666;
    font-style: italic;
  }
</style>