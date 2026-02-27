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
    <!-- <a href={base + p.url} -->
    <a href={p.url.startsWith("http") ? p.url : base + p.url}
        class:current={p.url === "/" // is this link the home page?
        ? $page.url.pathname === (base + "/") // if yes - set current = true if the path name matches. Else, set current = true if the path name starts correctly
        : $page.url.pathname.startsWith(base + p.url)}
        target={p.url.startsWith("http") ? "_blank": null}
    >
     {p.title}
    </a>
  {/each}
</nav>

<slot />
