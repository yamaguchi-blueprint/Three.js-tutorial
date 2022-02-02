<script setup lang="ts">
import { ref, onMounted,computed, render } from "vue";
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
        // 背景のグリッドの追加
        scene.add(new GridHelper(30));
        scene.background = new Color(0xcccccc);
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
        renderer.setClearColor(new Color(0xEEEEEE))
        container.value.appendChild(renderer.domElement);
         // 球体の追加
        const sphere = createSphere();
        scene.add(sphere);
        // 座標軸の追加
        const axes = new AxesHelper(20)
        scene.add(axes)
        // 描画
        animate();
      }
    }

    const animate = () => {
      // カメラの位置パラメータ
      // let phi = 0;
      const frame = () => {
        // カメラの視点変更
        // controls.update();
        // 描画
        // カメラの視点変更
        // phi += 0.001 * Math.PI;
        // camera.position.set(10 * Math.cos(phi), 10, 10 * Math.sin(phi));
        // camera.lookAt(1, 0, 1);
        // 画面を更新
        requestAnimationFrame(frame);
        renderer.render(scene, camera);
      };
      frame();
    }

   // マウント時に初期化して描画
    onMounted(() => {
      init();
    });
  // Sphereの作成
const createSphere = (): Mesh => {
    const geometry = new SphereGeometry(3);
    // const planeGeometry = new PlaneGeometry(60,40,1,1);
    const material = new MeshLambertMaterial({ color: 0xffff00 });
    return new Mesh(geometry, material);
    };

</script>

<template>
<div ref="container" id="css">
   <h1>Hello Three!</h1>

</div>

</template>

<style scoped>
#css{
  max-width: 100vw;
  min-height: 100vh;

}

</style>
