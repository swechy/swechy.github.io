## HTML 代码

```html
<div class="su7">
  <div class="box" ref="box"></div>
  <div class="su7-logo">
    XIAOMI SU7
    <p>人车合一 我心澎湃</p>
  </div>
</div>
```

## JavaScript 代码

```js
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
async getThreeJs(color) {
  let scene, renderer, camera, controls;
  scene = new THREE.Scene();
  scene.background = new THREE.Color(0x808080);
  scene.fog = new THREE.Fog(0x808080, 10, 15);
  camera = new THREE.PerspectiveCamera(30, 2 / 1, 0.1, 1000);
  camera.position.set(5.03, 2.68, -4.48);
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.shadowMap.enabled = true;
  const box = this.$refs.box;
  renderer.setSize(box.clientWidth, box.clientWidth / 2);
  box.appendChild(renderer.domElement);
  renderer.render(scene, camera);
  controls = new OrbitControls(camera, renderer.domElement);
  controls.maxPolarAngle = (0.9 * Math.PI) / 2;
  controls.enableZoom = false;
  controls.zoomSpeed = 1;
  let directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(-4, 8, 4);
  directionalLight.castShadow = true;
  directionalLight.shadow.radius = 5;
  directionalLight.shadow.mapSize.width = 1024;
  directionalLight.shadow.mapSize.height = 1024;
  let hemisphereLight = new THREE.HemisphereLight(0xffffff, 0x808080, 3);
  hemisphereLight.position.set(5, 8, 0);
  scene.add(directionalLight);
  scene.add(hemisphereLight);
  const floorGeometry = new THREE.PlaneGeometry(100, 100);
  const floormaterial = new THREE.MeshPhysicalMaterial({
    side: THREE.DoubleSide,
    color: 0x808080,
    metalness: 0,
    roughness: 0.1,
  });
  const mesh = new THREE.Mesh(floorGeometry, floormaterial);
  mesh.receiveShadow = true;
  mesh.rotation.x = Math.PI / 2;
  scene.add(mesh);
  function animate() {
    controls.update();
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
  }
  const loader = new GLTFLoader();
  const protocol = window.location.protocol;
  const host = window.location.host;
  const baseURL = protocol + "//" + host + "/";
  loader.load(baseURL + "static/gltf/scene.gltf", (gltf1) => {
    this.model = gltf1.scene;
    gltf1.scene.traverse(function (child) {
      if (child.isMesh) {
        child.material.metalness = 0.8;
        child.material.roughness = 0.01;
        child.castShadow = true;
      }
      const bodyMaterial = new THREE.MeshPhysicalMaterial({
        color: 0xd9cee4,
        metalness: 0.8,
        roughness: 0.01,
        clearcoat: 1.0,
        clearcoatRoughness: 0.03,
      });
      if (
        child.isMesh &&
        (child.name.indexOf("Object_52") > -1 ||
          child.name.indexOf("Object_45") > -1 ||
          child.name.indexOf("Object_39") > -1 ||
          child.name.indexOf("Object_32") > -1 ||
          child.name.indexOf("Object_18") > -1)
      ) {
        child.material = bodyMaterial;
      }
    });
    this.model.position.y = 0.03
    scene.add(this.model);
    animate();
  });
}
```
