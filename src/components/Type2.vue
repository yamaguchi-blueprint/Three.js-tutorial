<script setup lang="ts">
import {ref,onMounted} from 'vue'
import Stats from 'three/examples/jsm/libs/stats.module'
import GUI from 'lil-gui'
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
        SpotLight,
        PlaneGeometry,
        Sphere,
        Plane,
        BoxGeometry
        } from "three";
        import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";


    // 描画するDOMの指定
    const container = ref();
     // Three.js
    const scene = new Scene();
    const camera = new PerspectiveCamera();
    const pointlLight = new PointLight();
    const spotLight = new SpotLight(0xffffff)
    const renderer = new WebGLRenderer();
    const controls = new OrbitControls(camera, renderer.domElement);
    const axes = new AxesHelper(20);
    const gui = new GUI();
    const stats = new Stats();



     // 初期化
    const init = () => {
        if (container.value instanceof HTMLElement) {
             // DOMのサイズを取得
        const { clientWidth, clientHeight } = container.value;


        camera.aspect = clientWidth / clientHeight,0.2,1000;
        camera.updateProjectionMatrix();
        camera.position.set(-30, 40, 30);
        camera.lookAt(0, 0, 0);

        spotLight.position.set(20, 30, -5);
        spotLight.castShadow = true;
        spotLight.shadow.mapSize.width = 4096;
        spotLight.shadow.mapSize.height =4096;
        scene.add(spotLight);

        renderer.setSize(clientWidth, clientHeight);
        renderer.setPixelRatio(clientWidth / clientHeight);
        renderer.setClearColor(new Color(0xEEEEEE));
        renderer.shadowMap.enabled = true;
        container.value.appendChild(renderer.domElement);

        scene.add(axes)

        const cube = createCube()
        cube.position.x = -4;
        cube.position.y = 3;
        cube.position.z = 0;
        cube.castShadow = true;
        scene.add(cube)

        const sphere = createSphere()
        sphere.position.x = 14;
        sphere.position.y = 3;
        sphere.position.z = 0;
        sphere.castShadow = true;
        scene.add(sphere)

        const plane = createPlane();
        plane.rotation.x = -0.5* Math.PI
        plane.position.x = 15;
        plane.position.y = 0;
        plane.position.z = 0
        plane.receiveShadow = true;
        scene.add(plane);

        stats.showPanel(0);
        document.body.appendChild(stats.dom);

        // lil-gui
        const controls = {
        rotationSpeed: 0.02,
        bouncingSpeed: 0.03,
        };

        gui.add( controls, 'rotationSpeed', 0,0.1);
        gui.add( controls, 'bouncingSpeed', 0,0.5);


         // アニメーション
        const frameCube = () => {
            stats.begin();
            cube.rotation.y += controls.rotationSpeed;
            cube.rotation.x += controls.rotationSpeed;
            cube.rotation.z += controls.rotationSpeed;
            stats.end();
            requestAnimationFrame(frameCube)
            renderer.render(scene, camera);
        };
        frameCube();

        let step = 0;
        const frameSphere = () => {
            step += controls.bouncingSpeed;
            sphere.position.y = 2 + (10 * Math.abs(Math.sin(step)));
            sphere.position.x = 20 + (5 * (Math.cos(step)))
            requestAnimationFrame(frameSphere)
            renderer.render(scene, camera);
        };
        frameSphere();
        }
    }

    const colorFormats = {
	color1: '#777700',

    };
    gui.addColor( colorFormats, 'color1' );

    const createPlane = (): Mesh => {
    const geometry = new PlaneGeometry(60,20);
    const material = new MeshLambertMaterial({ color: 0xcccccc });
    return new Mesh(geometry, material);
    };
    const createCube = (): Mesh => {
    const cubeGeo = new BoxGeometry(4,4,4)
    const cubeMat = new MeshLambertMaterial({color: colorFormats.color1})
    return new Mesh(cubeGeo,cubeMat);
    };
    const createSphere = () : Mesh => {
    const sphereGeo = new SphereGeometry(3);
    const sphereMat = new MeshLambertMaterial({color: 0xff4500})
    return new Mesh(sphereGeo,sphereMat);
    }


    onMounted(() => {
    init();
    });
</script>
<template>

    <div ref="container" id="css"></div>


</template>

<style scoped>
#css{
  max-width: 150vw;
  min-height: 100vh;

}

</style>