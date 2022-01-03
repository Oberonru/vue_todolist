<template>
  <div class="wrapper">
    <h4 class="heading">Добавить ярлык</h4>

    <div class="tag_input_wrapper">
      <input class="tag_input" type="text"
             @keydown.enter="addTag"
             v-model="tagName"
             placeholder="введите название ярлыка">
      <img src="../images/search.svg" alt="img">
    </div>
    <ul class="tag_list">
      <li class="tag"
          v-for="tag in tags" :key="tag">
        {{ tag }}
      </li>
    </ul>
    <template v-if="tagName">
      <hr>
      <div class="showed_tag">
        <button class="add_btn"
                :style="{color: 'red' === verifyTagName(tagName)}"
                v-if="verifyTagName(tagName)"
                @click="addTag">
          +
        </button>
        <p class="create_tag_paragraph">Создать ярлык
          <span class="tag_name">
                "{{ tagName }}"
          </span>
        </p>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "TagPopUp",
  data() {
    return {
      tagName: "",
      tags: []
    }
  },
  computed: {

  },

  watch: {
    tagName: function (e) {
      console.log('tagname', e);
    }
  },

  methods: {
    addTag() {
      this.tags.push(this.tagName);
      this.tagName = "";
    },

    verifyTagName(tagName) {
      return tagName.length > 0 && tagName.length <= 30;
    }
  }
}
</script>

<style scoped>
.wrapper {
  position: absolute;
  z-index: 100501;
  width: 240px;
  margin: 0;
  min-height: 120px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
  background-color: lightgray;
}

.heading {
  padding: 3px 3px 0 5px;
}

.tag_input_wrapper {
  display: flex;
}

.tag_input {
  position: relative;
  outline: none;
  width: 85%;
  border: none;
  padding-left: 5px;
  background-color: lightgray;
}

.tag_input_wrapper img {
  position: absolute;
  width: 16px;
  height: 16px;
  top: 38px;
  left: 220px;
}

.tag_list {
  padding: 0 10px 0 20px;
}

.tag {
  list-style: none;
}

.tag:before {
  content: '';
  width: 80px;
  height: 80px;
  background-image: url("../images/square.svg");
}

.showed_tag {
  padding: 0 10px;
}

.add_btn {
  flex-grow: 1;
  all: unset;
  cursor: pointer;
  font-weight: bold;
  padding-right: 10px;
}

.create_tag_paragraph {
  display: inline;
  margin: 0;
}

.tag_name {
  word-wrap: break-word;
  overflow: hidden;
  text-overflow: ellipsis;
}

</style>