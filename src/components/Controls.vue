<template>
  <div>
    <button @click="captureInfo()">Capture</button>
    <div for="">
      <input type="checkbox" name="" v-model="debug" id="" />
      <span>debug</span>
    </div>
    <div for="">
      <span>this.im.camera.position.x</span>
      <input
        @change="updatePictureWithNew"
        type="number"
        v-model="im.camera.position.x"
      />
    </div>
    <label for="">
      <span>this.im.camera.position.y</span>
      <input
        @change="updatePictureWithNew"
        type="number"
        v-model="im.camera.position.y"
      />
    </label>
    <label for="">
      <span>this.im.camera.rotation.x</span>
      <input
        @change="updatePictureWithNew"
        type="number"
        v-model="im.camera.rotation.x"
      />
    </label>
    <label for="">
      <span>this.im.camera.rotation.y</span>
      <input
        @change="updatePictureWithNew"
        type="number"
        v-model="im.camera.rotation.y"
      />
    </label>

    <div class="controls">
      camera.position.x
      <input @change="setXVal" type="number" />
      camera.position.y
      <input @change="setYVal" type="number" />
      camera.position.z
      <input @change="setZVal" type="number" />
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  data: () => {
    return {
      im: null
    };
  },
  methods() {
    this.$emit("update", this.im);
  }
});
</script>
