<template>
  <div id="app">
    <FileSelector dataUrl="https://api-dev.reo.so/api/folderStructure" :onFilesSelect="onSelect" />
    <h3 v-if="files.length">Selected Files: </h3>
    <div :key="file" v-for="file in files">{{file}}</div>
  </div>
</template>

<script>
import FileSelector from './components/FileSelector.vue'

export default {
  name: 'App',
  components: {
    FileSelector
  },
  computed: {
    files: function() {
      return Object.values(this.selectedFiles).map(file => file.url);
    }
  },
  data: function() {
    return {
      selectedFiles: {}
    }
  },
  methods: {
    onSelect(selectedData) {
      this.selectedFiles = selectedData;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.d-flex {
  display: flex;
}
.justify-content-between {
  justify-content: space-between;
}
.align-items-center {
  align-items: center;
}
i {
  color: #595959;
}
.pointer {
  cursor: pointer;
}
button {
  color: #fff;
  background-color: #0074cc;
  border-radius: 6px;
  position: relative;
  padding: 0.75rem;
  border-color: #0074cc;
  border: 0px;
  cursor: pointer;
}
</style>
