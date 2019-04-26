<script>
	import { onMount } from "svelte";
	import names from "./data";
	import Combobox from "./Combobox.svelte";
	import Pokecard from "./Pokecard.svelte";

	let pokemon;

	async function hashChange() {
		// the poor man's router!
		const path = window.location.hash.slice(1);

		if (path.startsWith('/pokemon')) {
			const name = path.slice(9);
			console.log(name)
			pokemon = await fetch(`https://pokeapi.co/api/v2
/pokemon/${name}/`).then(r => r.json()).catch(console.error);

			window.scrollTo(0,0);
		} else {
			window.location.hash = '/';
		}
	}

	onMount(hashChange)
</script>

<svelte:window on:hashchange={hashChange}/>

<Combobox items={names} onChange={console.log} />
{#if pokemon}
  <Pokecard {pokemon} />
{/if}

<style>
	:global(*) {
	  margin: 0;
	  box-sizing: border-box;
	}

	:global(body) {
	  display: grid;
	  justify-items: center;
	  align-items: start;
	  padding-top: 6rem;
	  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
	    Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
	  color: hsl(220, 10%, 20%);
	  background-color: hsl(220, 10%, 96%);
	}
</style>