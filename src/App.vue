<template>
  <div class="container">
    <category-block :todoData="todoList"
                    @addCategory="showAddedCategory"
                    @filterUserInput="setUserFilter"
    />

    <div class="wrapper">
      <label for="todo_id" class="todo_input_label"> Your current todo</label>
      <textarea class="todo_input" name="" id="todo_id" v-model="userInput">
      </textarea>
      <button v-if="userInput.length > 2" v-on:click="add(userInput)">
        add...
      </button>
      <div class="todo">
        <ul class="todo_ul">
          <div
              class="todo_content"
              v-for="t in filterByCategory"
              :key="t.date"
              @click="this.select = t"
              :class="{ task_border: select === t }"
          >
            <p>{{ t.date }}</p>
            <p
                class="task"
                @click="t.isSelect = !t.isSelect"
                :class="{ task_paragraph: t.isSelect }"
            >
              {{ t.task }}
            </p>
            <!--                        <input class="changed_todo_input" type="text" :value="t.task">-->
            <!----------  navigation  --------------->
            <div class="navigation">
              <button
                  v-if="t.isSelect"
                  v-on:click="remove(t)"
                  :disabled="!t.isSelect"
              >
                <img
                    title="удалить"
                    class="remove_img"
                    src="./images/deleted-file.svg"
                    alt="удалить"
                />
              </button>

              <button @click="changeTodo()">
                <img
                    title="редактировать"
                    class="edit_img"
                    src="./images/edit.svg"
                    alt="редактирование"
                />
              </button>

              <button @click="popUpNow()">
                <img
                    title="задать категорию"
                    class="tags_img"
                    src="./images/vertical-dots.svg" alt="tags">

              </button>
            </div>
            <!----------//navigation  --------------->
          </div>
        </ul>
      </div>
    </div>
  </div>
  <BlobMenu/>
</template>

//[] Размеры каждой todo должны быть через константы, (препроцесооры или что то
еще), чтобы див navigation был внутри //[] При нажатии на карандаш - возможность
редактирования задачи

<script>
import CategoryBlock from "./components/CategoryBlock";
import BlobMenu from "./components/BlobMenu";

export default {
  name: "App",
  components: {
    CategoryBlock,
    BlobMenu
  },

  data() {
    return {
      userInput: "",

      todo: {
        task: "",
        date: "",
        isSelect: false,
        category: "default",
      },

      todoList: [],

      select: null,

      over: false,

      filterInput: "",
    };
  },

  created() {
    this.todoList = JSON.parse(localStorage.getItem("todoLIst")) ?? [];
  },

  watch: {
    filterInput: function () {
      return this.todoList.filter((todo) =>
          todo.category.startsWith(this.filterInput)
      );
    },
  },

  computed: {
    filterByCategory() {
      return this.todoList.filter((element) =>
          element.category.startsWith(this.filterInput)
      );
    },
  },

  methods: {
    add() {
      this.todoList.push({
        ...this.todo,
        task: this.userInput,
        date: new Date().toDateString(),
      });
      localStorage.setItem("todoLIst", JSON.stringify(this.todoList));
      console.log(this.todoList);
      this.userInput = "";
    },

    remove(todo) {
      this.todoList = this.todoList.filter((t) => t.task !== todo.task);
      localStorage.setItem("todoLIst", JSON.stringify(this.todoList));
    },

    showAddedCategory(value) {
      console.log("show", value);
    },
    setUserFilter(value) {
      this.filterInput = value;
    },

    popUpNow() {
      console.log('popUPNOW');
      return document.createElement('div');
    }
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

.container {
  margin: 0 auto;
  display: flex;
  max-width: 1200px;
}

.wrapper {
  flex-grow: 2;
  /*max-width: 600px;*/
  margin: 0 auto;
}

.todo_input_label {
  margin: 0 auto;
}

.todo_input {
  width: 100%;
  min-height: 45px;
  box-sizing: border-box;
  font-size: 1.2rem;

  resize: vertical;
  padding: 5px 5px 0 8px;
  /*padding: 0 240px 0 10px;*/
}

.todo_ul {
  padding: 0;
}

.todo_content {
  position: relative;
  min-height: 120px;
  border: 1px solid darkgrey;
  border-radius: 5px;
  margin-bottom: 10px;
  word-wrap: break-word;
  padding: 0 5px;
}

.task_border {
  border: 2px solid lightcoral;
  border-radius: 5px;
}

.task_paragraph {
  text-decoration: line-through;
}

.task:hover {
  cursor: pointer;
}

.changed_todo_input {
  all: unset;
}

.navigation {
  position: absolute;
  left: 0;
  width: 100%;
  border-radius: 5px;
  padding: 0 10px;

  opacity: 1;
}

.navigation_active {
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

.tags_img {
  width: 24px;
  height: 24px;
}
</style>
