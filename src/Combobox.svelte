<script>
	import { fly } from "svelte/transition";

	export let items;
	export let onChange;

	let value = "";
	let isOpen = false;
	let index = -1;

	$: filtered = items.filter(item => item.includes(value));

	function selectItem(item) {
	  value = item;
	  index = -1;
	  isOpen = false;
	  if (onChange) onChange(item);
	}

	function close() {
	  index = -1;
	  isOpen = false;
	}

	function handleKeyDown(event) {
	  switch (event.key) {
	    case "ArrowDown":
	      index = (index + 1) % filtered.length;
	      break;
	    case "ArrowUp":
	      index = (index - 1 + filtered.length) % filtered.length;
	      break;
	    case "Enter":
	      selectItem(filtered[index]);
	      close();
	      break;
	    case "Escape":
	      close();
	      break;
	    default:
	      break;
	  }
	}
</script>

<svelte:window on:click={close} />
<div
	role="combobox"
	aria-autocomplete="list"
	aria-controls="suggestions"
	aria-expanded={isOpen}
	on:keydown={handleKeyDown}
>
	<label for="search">
		Search
		<input
			id="search"
			autocomplete="off"
			bind:value
			on:input={() => isOpen = !!value}
		  on:focus={() => isOpen = !!value}
		  on:click|stopPropagation
		/>
	</label>
	{#if isOpen}
		<ul
			role="listbox"
			id="suggestions"
			transition:fly="{{ y: -25, duration: 250 }}"
			aria-activedescendant="option-{index}"
		>
			{#each filtered as item, i (item)}
				<li
					role="option"
					id="option-{index}"
					class:active={index === i}
					on:click|stopPropagation={() => selectItem(item)}
				>
					{item}
				</li>
			{/each}
		</ul>
	{/if}
</div>

<style>
	div[role="combobox"] {
	  position: relative;
	}

	input {
	  display: block;
	  width: 26ch;
	  margin-top: 0.5rem;
	  border: 0;
	  border-radius: 0.125rem;
	  box-shadow: 0 2px 2px hsla(220, 10%, 40%, 0.2),
	    0 4px 8px hsla(220, 10%, 40%, 0.1);
	  padding: 0.5rem;
	  color: inherit;
	  font-size: inherit;
	  font-family: inherit;
	  background: #fff;
	}

	input:focus {
	  outline: 0;
	  box-shadow: 0 2px 4px hsla(220, 10%, 40%, 0.4),
	    0 6px 20px hsla(220, 10%, 40%, 0.3);
	}

	ul {
	  position: absolute;
	  top: 100%;
	  left: 0;
	  right: 0;
	  margin-top: 1rem;
	  box-shadow: 0 2px 4px hsla(220, 10%, 40%, 0.4),
	    0 6px 20px hsla(220, 10%, 40%, 0.3);
	  border-radius: 0.125rem;
	  padding: 0;
	  list-style: none;
	  background-color: #fff;
	}

	li {
	  padding: 0.5rem;
	  transition: 0.1s background-color ease-out;
	}

	li:hover {
	  background-color: hsl(220, 20%, 94%);
	}

	li.active {
	  background-color: hsl(220, 20%, 94%);
	}
</style>