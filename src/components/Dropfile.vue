<template>
  <div
    class="drop_area"
    @dragenter="dragEnter"
    @dragleave="dragLeave"
    @dragover.prevent
    @drop.prevent="dropFile"
    :class="{enter: isEnter}"
  >
    ファイルアップロード
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

const fileRead = (file) => {
  return new Promise((res, rej) => {
    try {
      const reader = new FileReader()
      reader.onload = res
      reader.readAsText(file)
    } catch (err) {
      rej(err)
    }
  })

}

export default Vue.extend({
  name: 'Dropfile',
  data() {
    return {
      isEnter: false
    }
  },
  methods: {
    dragEnter() {
      this.isEnter = true;
    },
    dragLeave() {
      this.isEnter = false;
    },
    async dropFile(e) {
      const files = e.dataTransfer.files
      const params = {}
      for (const f of files) {
        const res = await fileRead(f)
        params[f.name.replace(/\.json$/, '')] = JSON.parse(res.target.result).results
      }
      this.$emit('success', params)
      this.isEnter = false
    }
  }
});
</script>

<style scoped>
.drop_area {
  color: gray;
  font-weight: bold;
  font-size: 1.2em;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 200px;
  border: 5px solid gray;
  border-radius: 15px;
}

.enter {
  border: 10px dotted powderblue;
}
</style>