<template>
  <ul class="list" v-if="!isTagSelect">
    <li @click="addTag" class="li_item add">Добавить ярлык</li>
    <li @click="changeTag" class="li_item">Изменить ярлык</li>
    <li @click="remove" class="li_item">Удалить задачу</li>
  </ul>
  <add-tag-menu
      v-if="isTagSelect"/>

</template>

<script>
import AddTagMenu from "./AddTagMenu";

export default {
  components: {
    AddTagMenu
  },

  data() {
    return {
      tag: "",
      isTagSelect: false
    }
  },

  methods: {
    addTag() {
      this.isTagSelect = !this.isTagSelect;
    },

    changeTag() {
      console.log('change tag');
    },
    remove() {
      this.$emit('remove');
    }
  }
}
</script>
<style>
.list {
  position: absolute;
  z-index: 100500;
  width: 140px;
  margin: 0;
  padding: 0;
  list-style: none;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  overflow: hidden;
}

.li_item {
  padding: 10px;
  background-color: lightgray;
  cursor: pointer;
}

.li_item:hover {
  background-color: #838383;
}

.add {
  position: relative;
}
</style>