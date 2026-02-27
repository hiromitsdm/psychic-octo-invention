<script>
import { base } from "$app/paths";
import { page } from "$app/stores";

let pages = [
  {url: "/", title: "About"},
  {url: "/projects", title: "Projects"},
  {url: "/resume", title: "Resume"},
  {url: "/contact", title: "Contact"},
  {url: "https://github.com/hiromitsdm", title: "Github"},
];
</script>

<nav>
  {#each pages as p}
    <a href={base + p.url}
      class:current={true}
      target={p.url.startsWith("http") ? "_blank" : null}
    >{p.title}</a>
  {/each}
</nav>

<slot />
