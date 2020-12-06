<template>
  <div @keyup="keyUp" class="about">
    <canvas width="640" height="480" id="c" style="width: 100%"></canvas>
    <Controls
      :debug="debug"
      :displaydata="im"
      @capture="captureInfo()"
      @update="updatePictureFromControls"
    />
  </div>
</template>
<script>
import Controls from "@/components/Controls.vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

export default {
  components: {
    Controls,
  },
  static() {
    return {
      scene: new THREE.Scene(),
      camera: null,
      renderer: THREE.WebGLRenderer,
      factor: 0,
    };
  },
  data: function () {
    return {
      im: {
        camera: {
          position: {
            x: 0,
            y: 0,
            z: 0,
          },
          rotation: {
            x: 0,
            y: 0,
            z: 0,
          },
        },
        car: {
          position: {
            x: 0,
            y: 0,
            z: 0,
          },
          rotation: {
            x: 0,
            y: 0,
            z: 0,
          },
        },
      },
      debug: false,
      canvas: null,
      constrols: null,
      fov: 45,
      aspect: 2,
      near: 0.1,
      far: 100,
    };
  },
  methods: {
    captureInfo() {
      console.log(this.camera.position);
      console.log(this.camera.rotation);
      this.im.camera.position.x = this.camera.position.x;
      this.im.camera.position.y = this.camera.position.y;
      this.im.camera.position.z = this.camera.position.z;

      this.im.camera.rotation.x = this.camera.rotation.x * 10;
      this.im.camera.rotation.y = this.camera.rotation.y * 10;
      this.im.camera.rotation.z = this.camera.rotation.z * 10;

      this.im.car.position.x = this.root.position.x;
      this.im.car.position.y = this.root.position.y;
      this.im.car.position.z = this.root.position.z;

      this.im.car.rotation.x = this.root.rotation.x * 10;
      this.im.car.rotation.y = this.root.rotation.y * 10;
      this.im.car.rotation.z = this.root.rotation.z * 10;
    },
    makeSureFormCorrect() {
      this.captureInfo();
    },
    updatePictureFromControls(data) {
      this.camera.position.x = data.camera.position.x;
      this.camera.position.y = data.camera.position.y;
      this.camera.position.z = data.camera.position.z;

      this.camera.rotation.x = data.camera.rotation.x * 0.1;
      this.camera.rotation.y = data.camera.rotation.y * 0.1;
      this.camera.rotation.z = data.camera.rotation.z * 0.1;

      this.root.position.x = data.car.position.x;
      this.root.position.y = data.car.position.y;
      this.root.position.z = data.car.position.z;

      this.root.rotation.x = data.car.rotation.x * 0.1;
      this.root.rotation.y = data.car.rotation.y * 0.1;
      this.root.rotation.z = data.car.rotation.z * 0.1;
    },
    keyUp() {
      console.log("arguments");
    },
    mainMethod() {
      this.canvas = document.querySelector("#c");
      this.renderer = new THREE.WebGLRenderer({
        canvas: this.canvas,
        antialias: true,
      });
      this.camera = new THREE.PerspectiveCamera(
        this.fov,
        this.aspect,
        this.near,
        this.far
      );
      this.camera.position.set(0, 1000, 20);

      this.controls = new OrbitControls(this.camera, this.canvas);
      this.controls.target.set(0, 5, 0);
      //this.controls.update();

      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color("black");

      //white board
      {
        const planeSize = 40;

        const loader = new THREE.TextureLoader();
        const texture = loader.load(
          "https://threejsfundamentals.org/threejs/resources/images/checker.png"
        );
        texture.wrapS = THREE.RepeatWrapping;
        texture.wrapT = THREE.RepeatWrapping;
        texture.magFilter = THREE.NearestFilter;
        const repeats = planeSize / 2;
        texture.repeat.set(repeats, repeats);

        const planeGeo = new THREE.PlaneBufferGeometry(planeSize, planeSize);
        const planeMat = new THREE.MeshPhongMaterial({
          map: texture,
          side: THREE.DoubleSide,
        });
        const mesh = new THREE.Mesh(planeGeo, planeMat);
        mesh.rotation.x = Math.PI * -0.5;
        //this.scene.add(mesh);
      }
      //add lights
      {
        const skyColor = 0xb1e1ff; // light blue
        const groundColor = 0xb97a20; // brownish orange
        const intensity = 10;
        const light = new THREE.HemisphereLight(
          skyColor,
          groundColor,
          intensity
        );
        // this.scene.add(light);
      }
      //add lights
      {
        const color = 0xffffff;
        const intensity = 1;
        const light = new THREE.DirectionalLight(color, intensity);
        light.position.set(5, 10, 2);
        this.scene.add(light);
        this.scene.add(light.target);
        const light0 = new THREE.AmbientLight(0xffffff, 1);
        //this.scene.add(light0);
        const light2 = new THREE.PointLight(0xc4c4c4, 10);
        light2.position.set(500, 100, 0);
        this.scene.add(light2);
        const light3 = new THREE.PointLight(0xc4c4c4, 10);
        light3.position.set(0, 100, -500);
        this.scene.add(light3);
        const light4 = new THREE.PointLight(0xc4c4c4, 10);
        light4.position.set(-500, 300, 500);
        this.scene.add(light4);
      }
      // load gltf
      {
        const gltfLoader = new GLTFLoader();
        console.log("glts");
        gltfLoader.load("cars/mazda_rx-7_fc/scene.gltf", (gltf) => {
          this.root = gltf.scene;
          console.log(this.root);
          this.scene.add(this.root);
          console.log(this.dumpObject(this.root).join("\n"));
          //cars = root.getObjectByName('RootNode');

          // compute the box that contains all the stuff
          // from root and below
          const box = new THREE.Box3().setFromObject(this.root);

          const boxSize = box.getSize(new THREE.Vector3()).length();
          const boxCenter = box.getCenter(new THREE.Vector3());

          // set the camera to frame the box
          this.frameArea(boxSize * 0.5, boxSize, boxCenter, this.camera);

          // update the Trackball controls to handle the new size
          this.controls.maxDistance = boxSize * 10;
          this.controls.target.copy(boxCenter);
          this.controls.update();
        });
      }
    },
    resizeRendererToDisplaySize(renderer) {
      const canvas = this.renderer.domElement;
      const width = canvas.clientWidth;
      const height = canvas.clientHeight;
      const needResize =
        this.canvas.width !== width || canvas.height !== height;
      if (needResize) {
        renderer.setSize(width, height, false);
      }
      return needResize;
    },
    frameArea(sizeToFitOnScreen, boxSize, boxCenter, camera) {
      const halfSizeToFitOnScreen = sizeToFitOnScreen * 0.5;
      const halfFovY = THREE.MathUtils.degToRad(camera.fov * 0.5);
      const distance = halfSizeToFitOnScreen / Math.tan(halfFovY);
      // compute a unit vector that points in the direction the camera is now
      // in the xz plane from the center of the box
      const direction = new THREE.Vector3()
        .subVectors(camera.position, boxCenter)
        .multiply(new THREE.Vector3(1, 0, 1))
        .normalize();

      // move the camera to a position distance units way from the center
      // in whatever direction the camera was from the center already
      camera.position.copy(direction.multiplyScalar(distance).add(boxCenter));

      // pick some near and far values for the frustum that
      // will contain the box.
      camera.near = boxSize / 100;
      camera.far = boxSize * 100;

      camera.updateProjectionMatrix();

      // point the camera to look at the center of the box
      camera.lookAt(boxCenter.x, boxCenter.y, boxCenter.z);
    },
    render(time) {
      time *= 0.001;
      if (this.resizeRendererToDisplaySize(this.renderer)) {
        const canvas = this.renderer.domElement;
        this.camera.aspect = canvas.clientWidth / canvas.clientHeight;
        this.camera.updateProjectionMatrix();
      }

      // if (cars) {
      //   for (const car of cars.children) {
      //     car.rotation.y = time;
      //   }
      // }

      this.renderer.render(this.scene, this.camera);

      requestAnimationFrame(this.render);
    },
    dumpObject(obj, lines = [], isLast = true, prefix = "") {
      const localPrefix = isLast ? "└─" : "├─";
      lines.push(
        `${prefix}${prefix ? localPrefix : ""}${obj.name || "*no-name*"} [${
          obj.type
        }]`
      );
      const newPrefix = prefix + (isLast ? "  " : "│ ");
      const lastNdx = obj.children.length - 1;
      obj.children.forEach((child, ndx) => {
        const isLast = ndx === lastNdx;
        this.dumpObject(child, lines, isLast, newPrefix);
      });
      return lines;
    },
    loop() {
      this.renderer.render(this.scene, this.camera);
      requestAnimationFrame(() => {
        this.loop();
        if (this.debug) {
          this.captureInfo();
        }
      });
    },
    handleKeysMethods() {
      const keys = ["ArrowUp", "ArrowDown", "ArrowRight", "ArrowLeft"];

      document.addEventListener(
        "keydown",
        (event) => {
          const keyName = event.key;

          //console.log(keyName)

          if (keyName === keys[0]) {
            console.log("up");
            this.camera.rotation.x -= 0.001;
            this.camera.position.y -= 1;

            return;
          }

          if (keyName === keys[1]) {
            console.log("up");
            this.camera.rotation.x += 0.001;
            this.camera.position.y += 1;

            return;
          }

          if (keyName === keys[2]) {
            console.log("right");
            //camera.rotation.y -= 0.01;
            this.camera.rotation.y -= 0.001;
            this.camera.position.x += 1;

            return;
          }
          if (keyName === keys[3]) {
            console.log("left");
            //camera.rotation.y += 0.01;
            this.camera.rotation.y += 0.001;
            this.camera.position.x -= 1;

            return;
          }
        },
        false
      );
    },
  },
  mounted() {
    this.mainMethod();
    this.loop();
    this.camera.position.set(0, 1000, 20);
    this.handleKeysMethods();
  },
};
</script>
