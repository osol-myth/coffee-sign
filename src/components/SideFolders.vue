<template>
  <div class="folder-items">
    <div class="folder-item" v-for="(folder, index) in folders" :key="index">
      <div class="folder-name" v-b-toggle="layer_id+'/'+index">
        <div>
          <i class="fa fa-caret-down toggle-icon" v-if="folder.children && folder.children.length > 0"/>
          <span class="space-div" v-if="!folder.children || folder.children.length <= 0"/>
          <i class="fa fa-folder-open" /> {{folder.name}}
        </div>
         <b-dropdown variant="link" left toggle-class="text-decoration-none" no-caret >
            <template slot="button-content">
              <i class="fa fa-ellipsis-v folder-menu" />
            </template>
            <b-dropdown-item v-on:click="rename(index, folder.name)">Rename</b-dropdown-item>
            <b-dropdown-item v-on:click="move(index)">Move</b-dropdown-item>
            <b-dropdown-item v-on:click="remove(index)">Delete</b-dropdown-item>
            <b-dropdown-item v-on:click="newFolder(index)">New Subfolder</b-dropdown-item>
         </b-dropdown>
      </div>
      <b-collapse v-if="folder.children && folder.children.length > 0" :id="layer_id+'/'+index">
        <SideFolders :folders="folder.children" :layer_id="layer_id+'/'+index"/>
      </b-collapse>
    </div>
  </div>
</template>

<script>
export default {
  name: "SideFolders",
  props: {
    folders: {
      type: Array,
      required: true,
      default: () => []
    },
    layer_id: {
      type: String,
      required: true,
      default: ""
    }
  },
  methods: {
    rename(index, cur_name) {
      this.$root.$emit('renameFolder', this.layer_id+'/'+index, cur_name);
    },
    move(index) {
      this.$root.$emit('moveFolder', this.layer_id+'/'+index);
    },
    remove(index) {
      this.$root.$emit('removeFolder', this.layer_id, index);
    },
    newFolder(index) {
      this.$root.$emit('newFolder', this.layer_id+'/'+index);
    },
  }
};
</script>
<style lang="scss">
@import "./SideFolders.scss";
</style>
