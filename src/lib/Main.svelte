<script lang="ts">
  import Card from "./Card.svelte";
  import Notification from "./Notification.svelte";
  export let secret;
  let value = "";
  let result: Promise<any>;
  let bound_button = false;
  function handleSubmit() {
    result = getResult();
  }
  $: is_bound = bound_button ? "is-loading" : "";

  function toggle_result() {
    bound_button = !bound_button;
  }

  async function getResult(): Promise<any> {
    toggle_result();
    const response = await fetch(`${secret.API_URI}?key=${secret.API_KEY}`, {
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      method: "POST",
      body: JSON.stringify({ text: value }),
    });

    if (!response.ok) {
      const message = `An error has occured: ${response.status} ${response}`;
      toggle_result();
      throw new Error(message);
    }
    toggle_result();
    return response.json();
  }
</script>

<div class="container">
  <div class="form">
    <form on:submit|preventDefault={handleSubmit} value="tldr-form">
      <textarea
        class="textarea is-primary has-fixed-size"
        bind:value
        placeholder="Enter text to summarize... (longer is better)"
        rows="10"
      />
      <button
        class="button is-primary {is_bound}"
        type="submit"
        disabled={!value}>TLDRify</button
      >
    </form>
  </div>

  {#if result === undefined}
    <p />
  {:else}
    {#await result then verified}
      <Card data={verified.summary} />
    {:catch error}
    <div class="notification-styled">
      <Notification type="is-danger">
        {error}
      </Notification>
      </div>
    {/await}
  {/if}
</div>

<style>
  button {
    float: right;
  }
  .container {
    height: 85%;
  }
  .form {
    margin: 5%;
  }
  .form textarea {
    margin-bottom: 3%;
  }
  .notification-styled {
    position: absolute;
    right: 0;
    bottom: 0;
  }
</style>
