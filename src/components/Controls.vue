<template>
  <div>
    <div class="top-controls">
      <button @click="captureInfo">Capture</button>
      <div for="">
        <input type="checkbox" name="" v-model="debug" id="" />
        <span>debug</span>
      </div>
    </div>

    <div class="grid-camera-car">
      <div>
        <h2>Camera</h2>
        <div>
          <p>position: (x, y, z)</p>
          <div class="inputs">
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.position.x"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.position.y"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.position.z"
            />
          </div>

          <p>rotation: (x, y, z)</p>
          <div class="inputs">
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.x"
              step="1"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.y"
              step="1"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.z"
              step="1"
            />
          </div>
        </div>
      </div>
      <div>
        <h2>Car</h2>
        <div>
          <p>position: (x, y, z)</p>
          <div class="inputs">
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.car.position.x"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.car.position.y"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              step="1"
              v-model="displaydata.car.position.z"
            />
          </div>
          <p>rotation: (x, y, z)</p>
          <div class="inputs">
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.car.rotation.x"
              step="1"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.car.rotation.y"
              step="1"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              step="1"
              v-model="displaydata.car.rotation.z"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    displaydata: {
      required: true,
    },
    debug: {
      required: true,
    },
  },
  data: () => {
    return {
      im: null,
    };
  },
  methods: {
    updatePictureWithNew() {
      this.$emit("update", this.displaydata);
    },
    captureInfo() {
      this.$emit("capture");
    },
  },
});
</script>
<style>
label span,
div span {
  color: #fff;
}
.grid-camera-car {
  display: grid;
  grid-template-columns: 1fr 1fr;
  width: 100%;
  overflow: hidden;
}
.grid-camera-car div.inputs {
  display: block;
  grid-template-columns: 1fr 1fr 1fr;
}
h2 {
  color: #fff;
  font-size: 12px;
  margin-bottom: 0;
}
.grid-camera-car p {
  color: #fff;
  font-size: 12px;
  font-weight: bold;
  margin-top: 0;
}
.grid-camera-car input {
  max-width: 100px;
}
.top-controls {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 20px;
}
</style>
