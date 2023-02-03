<template>
  <div id="cont" ref="cont"></div>
</template>
<script>
import * as THREE from "three";
import { MapControls } from "three/examples/jsm/controls/OrbitControls";
export default {
  data() {
    return {
      scene: null,
      camera: null,
      renderer: null,
      controls: null,
    };
  },
  mounted() {
    this.Awake();
    let that = this;

    window.addEventListener("resize", onWindowResize, false);
    function onWindowResize() {
      that.camera.aspect = window.innerWidth / window.innerHeight;
      that.camera.updateProjectionMatrix();
      that.renderer.setSize(window.innerWidth, window.innerHeight);
    }
    onWindowResize();

    fetch("https://geo.datav.aliyun.com/areas_v3/bound/360111.json")
      .then((response) => response.json())
      .then((data) => console.log(data));
  },
  methods: {
    Awake() {
      const cont = document.getElementById("cont");

      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(0x222222);

      this.camera = new THREE.PerspectiveCamera(
        25,
        window.clientWidth / window.clientHeight,
        1,
        100
      );
      this.camera.position.set(8, 4, 0);

      const light0 = new THREE.AmbientLight(0xfafafa, 0.25);
      const light1 = new THREE.PointLight(0xfafafa, 0.4);
      light1.position.set(200, 90, 40);
      const light2 = new THREE.AmbientLight(0xfafafa, 0.25);
      light2.position.set(200, 90, -40);
      this.scene.add(light0);
      this.scene.add(light1);
      this.scene.add(light2);

      const gh = new THREE.GridHelper(
        60,
        160,
        new THREE.Color(0x555555),
        new THREE.Color(0x333333)
      );
      this.scene.add(gh);

      const geometry = new THREE.BoxGeometry(1, 1, 1);
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
      const cube = new THREE.Mesh(geometry, material);
      this.scene.add(cube);

      // 渲染
      this.renderer = new THREE.WebGL1Renderer({ antialias: true });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.setSize(window.innerWidth, window.innerHeight);

      cont.appendChild(this.renderer.domElement);

      this.controls = new MapControls(this.camera, this.renderer.domElement);
      this.controls.enableDamping = true;
      this.controls.dampingFactor = 0.25;
      this.controls.screenSpacePanning = false;
      this.controls.maxDistance = 800;

      this.controls.update();
      this.Update();
    },
    Update() {
      requestAnimationFrame(this.Update);
      this.renderer.render(this.scene, this.camera);
      this.controls.update;
    },
  },
};
</script>

<style scoped></style>
