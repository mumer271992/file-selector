<template>
  <div class="file-selector">
    <button @click="toggleDropdown">Select Files</button>
    <Dropdown
      :data="folderStructure"
      v-if="showDropdown"
      :onClose="toggleDropdown"
      :onFilesSelect="onFilesSelect"
    />
  </div>
</template>

<script>
import Dropdown from './Dropdown.vue';

export default {
  name: 'FileSelector',
  props: [
    'onFilesSelect',
    'files',
    'dataUrl'
  ],
  components: {
    Dropdown
  },
  computed: {
    filesCount: function (){
      console.log(this.selectedFiles)
      return Object.keys(this.selectedFiles).length || ''
    },
    isRoot: function() {
      return this.stack.length <= 1;
    },
    heading: function () {
      return this.stack[this.stack.length - 1].name || 'Select files'
    }
  },
  created() {
    const url = this.dataUrl;
    fetch(url).then(res => res.json()).then(data => {
      this.folderStructure = data;
      this.stack.push(data);
    }).catch(err => {
      console.log(err);
    })
  },
  data() {
    return {
      folderStructure: {},
      showDropdown: false,
      stack: [],
      selectedFiles: {},
      allowedFileTypes: ['image/jpeg', 'image/jpg', 'image/png', 'application/pdf']
    }
  },
  methods: {
    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    },
    goInFolder(a) {
      this.stack.push(a);
    },
    selectFile(file) {
      if (!this.selectedFiles[file.id]) {
        this.$set(this.selectedFiles, file.id, file);
      } else {
        this.$delete(this.selectedFiles, file.id);
      }
    },
    shouldHide(file) {
      console.log(file.mimeType);
      return !this.allowedFileTypes.includes(file.mimeType);
    },
    back() {
      if (this.stack.length <= 1) return;
      this.stack.pop();
    },
    select() {
      console.log(Object.values(this.selectedFiles));
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.file-selector {
  display: inline-block;
  position: relative;
}
</style>
