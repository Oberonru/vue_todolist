<template>
  <div class="tag">
    <div class="input_element">
      <input type="text" v-model="tag" @keydown.enter="addTag()"/>
      <button @click="addTag()">add tag</button>
    </div>
    <ul>
      <li v-for="(tag, index) in tags" :key="index++">
        <p>{{ tag }}</p>
      </li>
    </ul>

    <div class="category_filter">
      <input type="text" id="category_filter" v-model="filter"/>
      <label for="category_filter">Filtred by category</label>
    </div>
  </div>
</template>

<script>

export default {
  name: "LeftColumn",
  components: {},

  data() {
    return {
      tags: [],
      tag: "",
      filter: ""
    };
  },

  props: {
    todoList: {
      type: Object,
      default() {
        return {};
      },
    },
  },

  watch: {
    filter: function () {
      this.$emit('getFilter', this.filter);
    }
  },

  methods: {
    addTag() {
      this.tags.push(this.tag);
      this.$emit("addCategory", this.tag);
      this.tag = "";
    },
  },
};
</script>

<style scoped>
.tag {
  min-height: 50vh;
  max-height: 95vh;
  position: relative;
  flex-grow: 1;
  background-color: #fcaeae;
  border-radius: 10px;
  margin-right: 10px;
}

.input_element {
  padding: 20px;
}

.input_element input {
  all: unset;
  border-bottom: 1px solid black;
  cursor: pointer;
}

.input_element button {
  margin-left: 10px;
  cursor: pointer;
}

.category_filter {
  position: absolute;
  bottom: 0;
  margin-top: 100%;
  padding: 20px;
}

.category_filter input {
  all: unset;
  border-bottom: 1px solid black;
}

.category_filter label {
  margin-left: 10px;
}
</style>
