<template>
    <header>
        <div>
            <nav>
                <RouterLink to="/">Home</RouterLink>
                <RouterLink to="/about">About</RouterLink>
            </nav>
        </div>
    </header>
    <RouterView />
</template>

<script setup lang="ts">
    import { RouterLink, RouterView } from 'vue-router';
</script>

<style scoped>
    header{
        width: 100%;
    }
    nav{
        display:grid;
        gap: 50px 100px;
        grid-template-columns: 10px 10px;
    }

</style>