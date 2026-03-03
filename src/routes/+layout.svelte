<script>
    import { base } from "$app/paths";
    import { page } from "$app/stores";

    let pages = [
        {url: "/", title: "About"},
        {url: "/projects", title: "Projects"},
        {url: "/resume", title: "Resume"},
        {url: "/contact", title: "Contact"},
        {url: "https://github.com/milintun", title: "Github"},
    ];

    let colorScheme = "light dark";
    let root = globalThis.document?.documentElement;
    $: root?.style.setProperty("color-scheme", colorScheme);

    let localStorage = globalThis.localStorage ?? {};

    if (localStorage.colorScheme) { // if localStorage has a colorScheme property
    colorScheme = localStorage.colorScheme; // override the default colorScheme
    }

    $: localStorage.colorScheme = colorScheme;

</script>

<label class="color-scheme-switch">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>

<nav>
    {#each pages as p}
    <a 
        href={base + p.url} 
        target={p.url.startsWith("http") ? "_blank" : null}
        class:current={p.url === "/" // is this link the home page?
            ? $page.url.pathname === (base + "/") // if yes - set current = true if the path name matches. Else, set current = true if the path name starts correctly
            : $page.url.pathname.startsWith(base + p.url)}
    >
        {p.title}
    </a>
    {/each}
</nav>

<slot />

<style>
    .color-scheme-switch {
        position: absolute;
        top: 1rem;
        right: 1rem;
        display: inline-flex;
        gap: 4px;
        font-size: 80%;
        font-family: inherit;
    }  

	nav {
		display: flex;
		margin-bottom: 2rem;
		border-bottom-width: 1px;
		border-bottom-style: solid;
		border-bottom-color: oklch(50% 10% 200 / 40%);
	}

	nav a {
		flex: 1;
		text-align: center;
		padding: 0.5em;
	}

	nav a:hover {
		border-bottom: 0.4em solid var(--color-accent);
		background-color: color-mix(in oklch, var(--color-accent), canvas 85%);;
	}

	:global(.current) {
		font-weight: bold;
		border-bottom: 0.4em solid oklch(50% 10% 200 / 40%);
	}
</style>