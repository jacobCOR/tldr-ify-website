<script lang="ts">
	import Card from './Card.svelte';
	import * as secret from '../secrets.json';
	import Navbar from './Navbar.svelte';
	import 'bulma/css/bulma.css';
	let value = "";
	let result: Promise<any>;
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

<Navbar/>

<div class="container">
	<h4>TL;DRify</h4>
	<div class="form">
	<form on:submit|preventDefault={handleSubmit} value="tldr-form">
		<textarea bind:value placeholder="Enter text to shorten..."></textarea>
		<button disabled={!value} type=submit>TL;DRify</button>
	</form>
</div>

	{#if result===undefined}
		<p></p>
	{:else}
		{#await result}
			<!-- <div class="spinner-border mt-5" role="status">
				<span class="sr-only">Loading...</span>
			</div> -->
			{:then verified}

			<Card data={verified.summary} />

			{:catch error}

			<Card data={error.message}/>
		{/await}
	{/if}
</div>

<style>
	textarea {
		resize: none;
		display: block;
		width: 500px;
		height: 250px;
		max-width: 100%;
		margin-left: auto;
   		margin-right: auto;
	}
	.container {
		display: flex;
  		justify-content: center;
		outline: dashed 1px black;
		flex-direction: column;
		align-items: center;
	}
	button {
		float: right;
	}
	.form {
		outline: 1px dashed red;
		display: block;
		width: 500px;
	}
</style>