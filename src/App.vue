<template>
  <div class="container">
    <left-column
        :todoData="todoList"
        @addCategory="showAddedCategory"
        @filterUserInput="setUserFilter"
    />

    <div class="wrapper">
      <label for="todo_id" class="todo_input_label"> Your current todo</label>
      <textarea class="todo_input" name="" id="todo_id" v-model="userInput">
      </textarea>
      <button class="add_btn" v-if="userInput.length > 2" v-on:click="add(userInput)">
        add...
      </button>
      <div class="todo_wrapper">
        <ul class="todo_list">
          <li
              class="todo"
              v-for="todo in todoList"
              :key="todo.date"
              @click="this.select = todo"
              :class="{ task_border: select === todo }"
          >
            <p>{{ todo.date }}</p>
            <p
                class="task"
                @click="todo.isSelect = !todo.isSelect"
                :class="{ task_paragraph: todo.isSelect }"
            >
              {{ todo.task }}
            </p>

            <!--                        <input class="changed_todo_input" type="text" :value="t.task">-->
            <!----------  navigation  --------------->
            <todo-navigation
                @removeTodo="remove(todo)"
                :currentTodo="todo"/>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

//[] Размеры каждой todo должны быть через константы, (препроцесооры или что то
еще), чтобы див navigation был внутри //[] При нажатии на карандаш - возможность
редактирования задачи

<script>
import LeftColumn from "./components/LeftColumn";
import TodoNavigation from "./components/TodoNavigation";

export default {
  name: "App",
  components: {
    LeftColumn,
    TodoNavigation
  },

  data() {
    return {
      userInput: "",

      todo: {
        task: "",
        date: "",
        isSelect: false,
        tag: "default",
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
          todo.tag.startsWith(this.filterInput)
      );
    },
  },

  computed: {
    // filterByCategory() {
    //   return this.todoList.filter((element) =>
    //       element.tag.startsWith(this.filterInput)
    //   );
    //   // return this.todoList;
    // },
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


    showAddedCategory(value) {
      console.log("show", value);
    },

    remove(todo) {
      this.todoList = this.todoList.filter((t) => t.task !== todo.task);
      localStorage.setItem("todoLIst", JSON.stringify(this.todoList));
    },


    setUserFilter(value) {
      this.filterInput = value;
      console.log('value', value);
    },
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

.add_btn {
  margin: 5px 0 10px 0;
}

.todo_wrapper {

}

.todo_list {
  padding: 0;
}

.todo {
  position: relative;
  min-height: 120px;
  border: 1px solid darkgrey;
  border-radius: 5px;
  margin-bottom: 10px;
  word-wrap: break-word;
  padding: 0 5px;
  list-style: none;
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

</style>
