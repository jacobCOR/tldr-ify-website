<script>
    import { fly } from 'svelte/transition';
    export let data;
</script>
    
<div class="card mt-5" transition:fly="{{ y: 150, duration: 300 }}">
    <div class="card-body">
        <textarea readonly>{data}</textarea>
    </div>
</div>

<style>
</style>