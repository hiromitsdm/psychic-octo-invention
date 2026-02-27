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

<style>
nav a.current {
    border-bottom: 0.4em solid oklch(80% 3% 200); /* Thick bottom border to highlight current page */
    padding-bottom: 0.1em; /* Reduce bottom padding to counter border height (0.5em - 0.4em = 0.1em) */
}

nav {
    display: flex;
    margin-bottom: 2em; /* Separate navigation from content below */
    border-bottom: 1px solid oklch(80% 3% 200); /* Subtle border under entire nav bar */
}

nav a {
    flex: 1;
    text-decoration: none; /* Remove underline */
    color: inherit; /* Use parent text color */
    text-align: center; /* Center the text */
    padding: 0.5em; /* Add spacing */
}

nav a:hover {
    border-bottom: 0.4em solid var(--color-accent); /* Use accent color on hover */
    padding-bottom: 0.1em; /* Reduce padding to counter border height */
}
</style>
