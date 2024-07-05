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
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x135C9C);

        const width = threeContainer.value.clientWidth;
        const height = threeContainer.value.clientHeight;

        const camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
        camera.position.z = 5;

        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(width, height);
        threeContainer.value.appendChild(renderer.domElement);

        // Adicionando sistema de partículas
        const particlesGeometry = new THREE.BufferGeometry();
        const particlesCount = 10000;

        const posArray = new Float32Array(particlesCount * 3);

        for (let i = 0; i < particlesCount * 3; i++) {
          posArray[i] = (Math.random() - 0.5) * 10;
        }

        particlesGeometry.setAttribute('position', new THREE.BufferAttribute(posArray, 3));

        const particlesMaterial = new THREE.PointsMaterial({
          size: 0.005,
          color: 0xffffff,
        });

        const particles = new THREE.Points(particlesGeometry, particlesMaterial);
        scene.add(particles);

        // Função de animação
        const animate = function () {
          requestAnimationFrame(animate);

          particles.rotation.y += 0.002;

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
  width: 100%;
  height: 100vh;
  position: relative;
}
</style>
