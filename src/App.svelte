<script lang="ts">
	import Card from './Card.svelte';
	import * as secret from '../secrets.json';
	let value = "";
	let result;
	let bound_button = false;
	function handleSubmit(){
		result = getResult()
		.catch( error => {
			console.log(error.message);
		});
	}


	async function getResult(): Promise<any> {
		const response = await fetch(`${secret.API_URI}?key=${secret.API_KEY}`,
		{
			headers: {
			'Accept': 'application/json',
			'Content-Type': 'application/json'
			},
			method: "POST",
			body: JSON.stringify({"text": value})
		});
		if(!response.ok){
			const message = `An error has occured: ${response.status}`;
    		throw new Error(message);			
		}
		return await response.json();
	}
</script>


<form on:submit|preventDefault={handleSubmit}>
	<textarea bind:value></textarea>
	<button disabled={!value} type=submit>TL;DR</button>
</form>

{#if result===undefined}
	<p></p>
{:else}
	{#await result}
		<div class="spinner-border mt-5" role="status">
			<span class="sr-only">Loading...</span>
		</div>
		{:then verified}

		<Card data={verified.summary} />

		{:catch error}

		<Card data={error.message}/>
	{/await}
{/if}

<style>
	textarea {
		display: block;
		width: 300px;
		max-width: 100%;
	}
</style>