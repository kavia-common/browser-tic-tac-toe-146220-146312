<script lang="ts">
    import '../app.css';
    let { children } = $props();
</script>

<main class="layout">
    {@render children()}
</main>

<style>
    .layout {
        min-height: 100vh;
        display: grid;
        place-items: center;
        padding: 24px;
    }
</style>
