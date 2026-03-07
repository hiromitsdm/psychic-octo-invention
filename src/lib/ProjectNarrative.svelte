<script>
  import projects from '$lib/projects.json';
</script>

<div class="narrative">
  {#each projects as project}
    <div class="narrative-item">
      <div class="narrative-text">
        <span class="year">{project.year}</span>
        <h3>{project.title}</h3>
        <p>{project.story}</p>
      </div>
      <div class="narrative-image">
        <img src={project.image} alt={project.title} />
      </div>
    </div>
  {/each}
</div>

<style>
  .narrative {
    display: flex;
    flex-direction: column;
    gap: 4rem;
    margin: 2rem 0;
  }

  .narrative-item {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    align-items: center;
  }

  .narrative-item:nth-child(even) .narrative-text {
    order: 2;
  }

  .narrative-item:nth-child(even) .narrative-image {
    order: 1;
  }

  .year {
    font-size: 0.85rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: light-dark(#888, #aaa);
  }

  .narrative-text h3 {
    margin: 0.25rem 0 0.75rem;
    font-size: 1.25rem;
  }

  .narrative-text p {
    line-height: 1.7;
    color: light-dark(#333, #ccc);
  }

  .narrative-image img {
    width: 100%;
    height: 280px;
    object-fit: cover;
    border-radius: 8px;
  }

  @media (max-width: 768px) {
    .narrative-item {
      grid-template-columns: 1fr;
    }

    .narrative-item:nth-child(even) .narrative-text {
      order: 0;
    }

    .narrative-item:nth-child(even) .narrative-image {
      order: 0;
    }
  }
</style>
