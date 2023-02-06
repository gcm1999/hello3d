<template>
  <div id="cont" ref="cont"></div>
</template>

<script>
import * as THREE from "three";
import {
  MapControls,
  OrbitControls,
} from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";
export default {
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      controls: null,
    };
  },
  created() {},
  mounted() {
    // console.log(GLTFLoader);
    const loader = new GLTFLoader();
    console.log(loader);
    // loader.load("../assets/111.glb");
    // 创建场景
    this.scene = new THREE.Scene();

    // 创建相机
    this.camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    // 设置相机位置
    this.camera.position.set(0, 0, 10);
    this.scene.add(this.camera);

    // 添加物体
    // 创建几何体
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshBasicMaterial({ color: 0xffff00 });
    // 根据几何体创建物体
    const cube = new THREE.Mesh(geometry, material);

    // 讲几何体添加到场景
    this.scene.add(cube);

    //添加坐标轴辅助器来作为参考，辅助器简单模拟3个坐标轴的对象。
    //红色代表 X 轴.绿色代表 Y 轴.蓝色代表 Z 轴。
    const axesHelper = new THREE.AxesHelper(5);
    this.scene.add(axesHelper);

    // 初始化渲染器
    this.renderer = new THREE.WebGLRenderer();
    // this.renderer.setPixelRatio(window.devicePixelRatio);
    // 设置渲染尺寸大小
    this.renderer.setSize(window.innerWidth, window.innerHeight);

    // 将webgl渲染的canvas内容添加到body
    this.$refs.cont.appendChild(this.renderer.domElement);

    // 创建轨道控制器
    this.controls = new OrbitControls(this.camera, this.renderer.domElement);

    this.render(cube);
  },
  methods: {
    render(cube) {
      cube.position.x += 0.01;
      if (cube.position.x >= 5) cube.position.x = 0;
      //如果后期需要控制器带有阻尼效果，或者自动旋转等效果，就需要加入controls.update()
      this.controls.update();
      //使用渲染器，通过相机将场景渲染进来
      this.renderer.render(this.scene, this.camera);
      //渲染下一帧的时候就会调用render函数
      requestAnimationFrame(() => {
        this.render(cube);
      });
    },
  },
};
</script>

<style scoped></style>
