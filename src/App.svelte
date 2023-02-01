<script>
	import ConnectedTree from "./ConnectedTree.svelte";
	import Collapse from "./Collapse.svelte"

	const id_prefix = 'svelvet-'
	let num = 2;
	function toggleByID(){
		const prefixedNum = id_prefix + num
		let el = document.getElementById(prefixedNum);

		let state = false;
		const default_style = "display:block";
		const toggle_style = "display:none";
		if (!('style' in el)){
			el.setAttribute("style", default_style)
		}

		el.style.display == 'none' ? el.style.display = 'block' : el.style.display = 'none';
	}

</script>

<main>
	<input type=number bind:value={num}>
	<button on:click={toggleByID} >
		toggle {num}
	</button>
	
	<ConnectedTree />
</main>


<style>

</style>