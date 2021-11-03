<template>
  <div class="dropdown">
    <div class="header">
      <div>
        <i
          class="fas fa-arrow-left pointer"
          @click="back"
          v-if="!isRoot"
        ></i>
        <span>{{heading}}</span>
      </div>
      <div>
        <i class="fas fa-times pointer" @click="clear"></i>
      </div>
    </div>
    <div class="files-folders-list">
      <FolderItem
        :key="folder.id"
        v-for="folder in folders"
        :folder="folder"
        :onClick="goInFolder"
      />
      <FileItem
        :key="file.id"
        v-for="file in files"
        :file="file"
        :onClick="selectFile"
        :isSelected="selectedFiles[file.id]"
      />
    </div>
    <div class="footer">
      <button @click="select" :disabled="!filesCount">Select {{filesCount}} files</button>
    </div>
  </div>
</template>

<script>
import FolderItem from './FolderItem.vue';
import FileItem from './FileItem.vue';

export default {
  name: 'Dropdown',
  props: [
    'data',
    'onClose',
    'onFilesSelect'
  ],
  components: {
    FolderItem,
    FileItem
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
      return this.stack[this.stack.length - 1]?.name || 'Select files'
    },
    files: function () {
      return !this.stack.length ? [] : this.stack[this.stack.length - 1].files;
    },
    folders: function () {
      return !this.stack.length ? [] : this.stack[this.stack.length - 1].folders;
    }
  },
  mounted() {
    this.stack.push(this.data);
  },
  data() {
    return {
      folderStructure: {},
      showDropdown: false,
      stack: [],
      selectedFiles: {}
    }
  },
  methods: {
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
      return !this.allowedFileTypes.includes(file.mimeType);
    },
    back() {
      if (this.stack.length <= 1) return;
      this.stack.pop();
    },
    select() {
      this.onFilesSelect(this.selectedFiles);
      this.onClose();
    },
    clear() {
      this.selectedFiles = {};
      this.stack = [this.data];
      this.onClose();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dropdown {
  display: flex;
  flex-direction: column;
  text-align: left;
  position: absolute;
  left: 0;
  width: 350px;
  height: 350px;
  box-shadow: 0 3px 10px rgb(0 0 0 / 0.2);
  padding: 1rem;
  border-radius: 8px;
  background-color: #fff;
}

.dropdown .files-folders-list {
  height: 280px;
  overflow: auto;
}


.dropdown .header {
  font-size: 18px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.dropdown .header span {
  margin-left: 0.5rem;
  font-weight: bold;
}

.dropdown .footer {
  display: flex;
  justify-content: flex-end;
}
</style>
