<script>
    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    // Sternenliste als writable store
    let stars = writable([]);

    // Daten laden
    onMount(async () => {
        const response = await fetch("/src/data/stardata.json");
        const data = await response.json();
        stars.set(data);
    });
</script>

<ul>
    {#each $stars as star}
        <li>
            <h2>{star.proper || star.bayer || star.flam}</h2>
            <p><strong>Constellation:</strong> {star.con}</p>
            <p><strong>Distance:</strong> {star.dist} parsec</p>
            <p><strong>Apparent Magnitude:</strong> {star.mag}</p>
            {#if star.wikiUrl}
                <p>
                    <a href={star.wikiUrl} target="_blank"
                        >More info on Wikipedia</a
                    >
                </p>
            {/if}
        </li>
    {/each}
</ul>

<style>
    body {
        font-family: Arial, sans-serif;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        background: #f9f9f9;
        margin: 10px 0;
        padding: 15px;
        border: 1px solid #ddd;
    }
    li h2 {
        margin: 0 0 10px 0;
    }
    li p {
        margin: 5px 0;
    }
</style>
