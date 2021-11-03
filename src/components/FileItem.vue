<template>
  <div class="file-item" @click="onClick(file)">
    <div class="d-flex justify-content-between align-items-center" v-if="allowedFileTypes.includes(file.mimeType)">
      <div class="icon" v-if="isPdf"><i class="far fa-file-pdf"></i></div>
      <img v-if="isImage" :src="file.url" />
      <div class="name">{{file.name}}</div>
    </div>
    <div class="selected" v-if="allowedFileTypes.includes(file.mimeType) && isSelected">
      <i class="fas fa-check-circle"></i>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FileItem',
  props: [
    'file',
    'onClick',
    'isSelected'
  ],
  computed: {
    idPdf: function() {
      return this.file.mimeType === 'application/pdf';
    },
    isImage: function() {
      return this.file.mimeType !== 'application/pdf';
    }
  },
  data: function() {
    return {
      allowedFileTypes: ['image/jpeg', 'image/jpg', 'image/png', 'application/pdf']
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.file-item {
  cursor: pointer;
  padding: 0.5rem 0.25rem;
  display: flex;
  justify-content: space-between;
  align-content: center;
  text-transform: capitalize;
}

.file-item .selected i{
  color: #0074cc;
}

.file-item .name{
  margin-left: 0.5rem;
}

.file-item .icon {
  width: 24px;
  height: 22px;
  padding: 0.15rem 0.25rem;
  background-color: #F5F5F5;
  border-radius: 4px;
  text-align: center;
}
</style>
