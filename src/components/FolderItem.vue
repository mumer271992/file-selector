<template>
  <div class="folder-item" @click="onClick(folder)">
    <div class="d-flex justify-content-between align-items-center">
      <div class="icon"><i class="far fa-folder"></i></div>
      <div class="name">{{folder.name}}</div>
    </div>
    <div>
      <span><i class="fas fa-chevron-right"></i></span>
    </div>
  </div>
</template>

<script>

export default {
  name: 'FolderItem',
  props: [
    'folder',
    'onClick'
  ]
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.folder-item {
  cursor: pointer;
  padding: 0.5rem 0.25rem;
  display: flex;
  justify-content: space-between;
  align-content: center;
  text-transform: capitalize;
}

.folder-item .selected i{
  color: #0074cc;
}

.folder-item .name{
  margin-left: 0.5rem;
}

.folder-item .icon {
  width: 24px;
  height: 22px;
  padding: 0.15rem 0.25rem;
  background-color: #F5F5F5;
  border-radius: 4px;
  text-align: center;
}
</style>
