<template>
  <div>
    <button @click="captureInfo">Capture</button>
    <div for="">
      <input type="checkbox" name="" v-model="debug" id="" />
      <span>debug</span>
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
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.y"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.z"
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
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.y"
            />
            <input
              @change="updatePictureWithNew"
              type="number"
              v-model="displaydata.camera.rotation.z"
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
      required: true
    },
    debug: {
      required: true
    }
  },
  data: () => {
    return {
      im: null
    };
  },
  methods: {
    updatePictureWithNew() {
      this.$emit("update", this.displaydata);
    },
    captureInfo() {
      this.$emit("capture");
    }
  }
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
}
.grid-camera-car div.inputs {
  display: block;
  grid-template-columns: 1fr 1fr 1fr;
}
h2{
  color: #fff;
  font-size: 12px;
  margin-bottom: 0;
}
.grid-camera-car p{
  color: #fff;
  font-size: 12px;
  font-weight: bold;
  margin-top: 0;
}
</style>
