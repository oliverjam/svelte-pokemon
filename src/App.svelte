<script>
	import Combobox from "./Combobox.svelte";

	let items = [
	  "apple",
	  "orange",
	  "banana",
	  "pineapple",
	  "dragonfruit",
	  "nectarine",
	  "grape"
	];
</script>

<Combobox {items} onChange={console.log} />

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