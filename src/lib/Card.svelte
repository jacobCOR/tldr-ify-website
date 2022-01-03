<script>
  import { fly } from "svelte/transition";
  export let data;
</script>

<div class="card-data">
  <h5 class="title is-3">Summary:</h5>
  <div class="card mt-5" transition:fly={{ y: 150, duration: 300 }}>
    <div class="card-body">
      <textarea class="textarea has-fixed-size is-primary" readonly
        >{data}</textarea
      >
    </div>
  </div>
</div>

<style>
  .card-data {
    margin: 5%;
  }
</style>
