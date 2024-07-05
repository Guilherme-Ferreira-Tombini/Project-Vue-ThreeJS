<!-- src/views/HomeView.vue -->
<template>
  <div>
    <ThreeScene />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import ThreeScene from '../components/ThreeScene.vue';

export default defineComponent({
  name: 'HomeView',
  components: {
    ThreeScene,
  },
});
</script>

<style scoped>

</style>
