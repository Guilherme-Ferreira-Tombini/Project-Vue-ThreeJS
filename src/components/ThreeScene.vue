<!-- src/components/ThreeView.vue -->
<template>
  <div ref="threeContainer" class="three-container"></div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';
import * as THREE from 'three';

export default defineComponent({
  name: 'ThreeView',
  setup() {
    const threeContainer = ref<HTMLElement | null>(null);

    onMounted(() => {
      if (threeContainer.value) {
        // Criação da cena
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x135C9C);

        // Criação da câmera
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);

        // Criação do renderizador
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(threeContainer.value.clientWidth, threeContainer.value.clientHeight);
        threeContainer.value.appendChild(renderer.domElement);

        // Criação de um cubo
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);

        camera.position.z = 5;

        // Função de animação
        const animate = function () {
          requestAnimationFrame(animate);

          cube.rotation.x += 0.01;
          cube.rotation.y += 0.01;

          renderer.render(scene, camera);
        };

        animate();
      }
    });

    return {
      threeContainer,
    };
  },
});
</script>

<style scoped>
.three-container {
  width: 50%;
  height: 250px;
}
</style>
