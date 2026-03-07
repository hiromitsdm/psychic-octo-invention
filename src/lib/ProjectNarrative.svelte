<script>
  import projects from '$lib/projects.json';
  import Scrolly from "svelte-scrolly";

  let scrollyProgress = 0;
  let sorted_projects = projects.sort((a, b) => a.year - b.year);
  let progressPerProject = 100 / sorted_projects.length;

  $: activeProjectIdx = Math.min(sorted_projects.length - 1, Math.floor(scrollyProgress / progressPerProject));
</script>

<Scrolly bind:progress={scrollyProgress}>
  {#each sorted_projects as project}
    <section>
      <div>
        <h3>{project.title}</h3>
        <p>{project.story}</p>
      </div>
    </section>
  {/each}

  <svelte:fragment slot="viz">
    {activeProjectIdx}
  </svelte:fragment>
</Scrolly>

<style>
  .story-step {
    padding: 4rem 2rem;
    min-height: 50vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .year {
    font-size: 0.85rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: light-dark(#888, #aaa);
  }

  .story-step h3 {
    margin: 0.25rem 0 0.75rem;
    font-size: 1.25rem;
  }

  .story-step p {
    line-height: 1.7;
    color: light-dark(#333, #ccc);
    max-width: 40ch;
  }

  .viz {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .viz img {
    width: 100%;
    height: 70vh;
    object-fit: cover;
    border-radius: 8px;
  }
</style>
