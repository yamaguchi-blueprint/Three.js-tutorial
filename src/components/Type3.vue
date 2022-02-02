<script setup lang="ts">
import {ref,onMounted} from 'vue'
import { GridHelper,
        Color,
        PerspectiveCamera,
        Scene,
        WebGLRenderer,
        AxesHelper,
        Mesh,
        SphereGeometry,
        MeshBasicMaterial,
        MeshLambertMaterial,
        PointLight,
        PlaneGeometry,
        Sphere
        } from "three";
        import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
    // 描画するDOMの指定
    const container = ref();
     // Three.js
    const scene = new Scene();
    const camera = new PerspectiveCamera();
    const light = new PointLight();
    const renderer = new WebGLRenderer();
    const controls = new OrbitControls(camera, renderer.domElement);
     // 初期化
    const init = () => {
        if (container.value instanceof HTMLElement) {
             // DOMのサイズを取得
        const { clientWidth, clientHeight } = container.value;
        // カメラの設定
        camera.aspect = clientWidth / clientHeight;
        camera.updateProjectionMatrix();
        camera.position.set(10, 10, 0);
        camera.lookAt(0, 0, 0);
         // ライトの設定
        light.color.setHex(0xffffff);
        light.position.set(10, 10, 0);
        scene.add(light);
        // rendererの設定
        renderer.setSize(clientWidth, clientHeight);
        renderer.setPixelRatio(clientWidth / clientHeight);
        container.value.appendChild(renderer.domElement);
        }
        animate();
    }
    const animate = () => {
        const frame = () => {
            requestAnimationFrame(frame);
            renderer.render(scene, camera);
        };
        frame();
    }
    onMounted(() => {
      init();
    });
      // Sphereの作成
    const createSphere = (): Mesh => {
    const geometry = new SphereGeometry(3);
    const material = new MeshLambertMaterial({ color: 0xffff00 });
    return new Mesh(geometry, material);
    };




</script>
<template>

        <div ref="container"></div>


</template>