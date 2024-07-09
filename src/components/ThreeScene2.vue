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
        camera.position.z = 50;

        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(width, height);
        threeContainer.value.appendChild(renderer.domElement);

        // Luz para iluminar o sistema solar
        const pointLight = new THREE.PointLight(0xffffff, 1.5, 1000);
        pointLight.position.set(0, 0, 0);
        scene.add(pointLight);

        // Adicionando luz ambiente
        const ambientLight = new THREE.AmbientLight(0xFFFFFF); // luz fraca
        scene.add(ambientLight);

        // Função para criar um planeta
        const createPlanet = (size: number, color: number, distance: number) => {
          const geometry = new THREE.SphereGeometry(size, 32, 32);
          const material = new THREE.MeshStandardMaterial({ color });
          const planet = new THREE.Mesh(geometry, material);
          const planetGroup = new THREE.Group();
          planetGroup.add(planet);
          planet.position.x = distance;
          scene.add(planetGroup);
          return planetGroup;
        };

        // Criando o Sol
        const sun = createPlanet(5, 0xffff00, 0);

        // Criando os planetas
        const planets = [
          createPlanet(1, 0xaaaaaa, 10), // Mercúrio
          createPlanet(1.5, 0xffd700, 15), // Vênus
          createPlanet(1.5, 0x0000ff, 20), // Terra
          createPlanet(1, 0xff0000, 25), // Marte
          createPlanet(3, 0xffa500, 35), // Júpiter
          createPlanet(2.5, 0xf0e68c, 45), // Saturno
          createPlanet(2, 0x00ffff, 55), // Urano
          createPlanet(1.5, 0x0000ff, 65) // Netuno
        ];

        // Função de animação
        const animate = function () {
          requestAnimationFrame(animate);

          // Rotação do sol
          sun.rotation.y += 0.004;

          // Orbitando os planetas
          planets.forEach((planetGroup, index) => {
            planetGroup.rotation.y += 0.01 * (index + 1);
          });

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
