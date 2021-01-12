<template>
    <div id="app">

    </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { OBJLoader } from 'three/examples/jsm/loaders/OBJLoader.js'
import { MTLLoader } from 'three/examples/jsm/loaders/MTLLoader.js'

export default {
    name: 'App',
    mounted() {
        const scene = new THREE.Scene();

        // 创建渲染器
        const renderer = new THREE.WebGLRenderer();
        renderer.setClearColor('#eeeeee', 1)
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // const width = window.innerWidth; //窗口宽度
        // const height = window.innerHeight; //窗口高度
        // const k = width / height; //窗口宽高比
        // const s = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
        // 创建相机对象(正交投影相机)
        // const camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
        // camera.position.set(200, 300, 200)
        // camera.lookAt(scene.position);

        // 创建透视投影相机
        const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
        camera.position.set(0, 0, 200)
        camera.lookAt(scene.position)

        // 创建光
        const pointLight = new THREE.PointLight(0xffffff);
        pointLight.position.set(200, 300, 400);
        scene.add(pointLight);
        // 创建环境光
        const ambientLight = new THREE.AmbientLight(0x666666);
        scene.add(ambientLight);


        const axisHelper = new THREE.AxesHelper(250);
        scene.add(axisHelper);

        function render() {
            renderer.render(scene, camera);
        }

        // 设置鼠标事件
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.addEventListener('change', render)

        const objLoader = new OBJLoader();//obj加载器
        const mtlLoader = new MTLLoader();//材质文件加载器
        mtlLoader.load('/obj/jigui.mtl', function (materials) {
            // obj的模型会和MaterialCreator包含的材质对应起来
            objLoader.setMaterials(materials)
            objLoader.load('/obj/jigui.obj', function (obj) {// url自己定义
                // 执行缩放，模型太小了
                obj.scale.set(30, 30, 30)
                scene.add(obj);// 返回的组对象插入场景中
                setTimeout(() => {
                    render()
                }, 100);
            }, undefined, function (err) {
                console.error(err)
            });
        });


        const objLoader2 = new OBJLoader();//obj加载器
        const mtlLoader2 = new MTLLoader();//材质文件加载器
        mtlLoader2.load('/obj/huawei.mtl', function (materials) {
            // obj的模型会和MaterialCreator包含的材质对应起来
            objLoader2.setMaterials(materials)
            objLoader2.load('/obj/huawei.obj', function (obj) {// url自己定义
                // 执行缩放，模型太小了
                obj.scale.set(30, 30, 30)
                obj.position.set(0, 10, 0)
                scene.add(obj);//返回的组对象插入场景中
                setTimeout(() => {
                    render()
                }, 100);
            }, undefined, function (err) {
                console.error(err)
            });
        });


    }
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
}
#app {
    width: 100%;
    height: 100%;
}
</style>
