<script>
  export let pokemon = {}
</script>

<img src={pokemon.sprites.front_default} alt="Pikachu's front sprite">
<h2>#{pokemon.id} {pokemon.name}</h2>

<style>
	h2 {
	  text-transform: capitalize;
	}
</style>