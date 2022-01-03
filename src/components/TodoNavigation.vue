<template>
  <div class="navigation">
    <button
        v-if="currentTodo.isSelect"
    >
      <img
          @click="remove(currentTodo)"
          title="удалить"
          class="remove_img"
          src="../images/deleted-file.svg"
          alt="удалить"
      />
    </button>

    <button>
      <img
          title="редактировать"
          class="edit_img"
          src="../images/edit.svg"
          alt="редактирование"
      />
    </button>

    <button class="toggleTagsMenu_btn" @click="toggleTagsMenu">
      <img
          title="задать категорию"
          class="tags_img"
          src="../images/vertical-dots.svg" alt="tags">
    </button>
    <tag-menu v-if="isTagsMenuActive"/>
  </div>
</template>

<script>
import TagMenu from "./TagMenu";

export default {
  name: "TodoNavigation",
  components: {
    TagMenu,
  },
  data() {
    return {
      isTagsMenuActive: false
    }
  },
  props: {
    currentTodo: {
      type: Object,
      default() {
        return {}
      }
    },
  },

  methods: {
    toggleTagsMenu() {
      this.isTagsMenuActive = !this.isTagsMenuActive;
    },

    addTag() {},

    remove(todo) {
      console.log('method remove');
      this.todoList =  this.todoList.filter((t) => t.task !== todo.task);
      localStorage.setItem("todoLIst", JSON.stringify(this.todoList));
    },
  }
}
</script>

<style scoped>
.navigation {
  position: absolute;
  left: 0;
  width: 100%;
  border-radius: 5px;
  padding: 0 10px;
  opacity: 1;
}

.remove_img {
  width: 24px;
  height: 24px;
}

.edit_img {
  width: 24px;
  height: 24px;
}

.toggleTagsMenu_btn {
  position: relative;
}

.tags_img {
  width: 24px;
  height: 24px;
}
</style>