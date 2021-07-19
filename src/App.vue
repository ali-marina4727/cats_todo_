<template>
  <div>
    <h1>Мяу-лист для кота</h1>

    <section>
      <div>
        <h3>Todo List</h3>
        <h4>отсортировать</h4>
        <CatSelector :categories="categories" @on-select="onSelectCategory" />
        <AddItem @on-task="onAddTask" :categories="categories" />
        <TodoList
          :todos="filteredTasks"
          :editedTask="editedTask"
          @on-delete="onDelete"
          @on-edit="onEdit"
          @on-edit-done="onEditDone"
          @on-edit-cancel="onEditCancel"
          @on-done="onTaskDone"
        />
      </div>
    </section>
  </div>
</template>

<script>
import TodoList from "./components/TodoList.vue";
import CatSelector from "./components/CategorySelector";
import AddItem from "./components/ItemAddForm";

export default {
  name: "App",
  components: {
    TodoList,
    CatSelector,
    AddItem,
  },
  data() {
    return {
      nextId: 6,
      nextIdPost: 2,
      todos: [
        { id: 1, label: "Выпрашивать еду", category_id: 2, isDone: false },
        { id: 2, label: "Шипеть", category_id: 1, isDone: false },
        {
          id: 3,
          label: "Высокомерно не давать себя почесать ",
          category_id: 1,
          isDone: false,
        },
        { id: 4, label: "Ластиться", category_id: 1, isDone: false },
        { id: 5, label: "Поймать мышь", category_id: 2, isDone: false },
        { id: 6, label: "Спать", category_id: 3, isDone: false },
        { id: 7, label: "Тыгдык", category_id: 3, isDone: false },
        {
          id: 8,
          label: "Залезть на штору и мяукать, чтобы сняли",
          category_id: 3,
          isDone: false,
        },
      ],
      categories: [
        { id: 0, name: "Все" },
        { id: 1, name: "Семья" },
        { id: 2, name: "Кушоть" },
        { id: 3, name: "Переривы" },
      ],
      label: "",
      editedTask: null,
      onSelectedCategId: 0,
    };
  },
  computed: {
    filteredTasks() {
      if (!this.onSelectedCategId) {
        return this.todos;
      }
      let todos = this.todos.filter(
        (item) => item.category_id === this.onSelectedCategId
      );
      return todos;
    },
  },
  methods: {
    onAddTask(label, categoryId, isDone) {
      if (label) {
        const newTodo = {
          id: this.nextId++,
          label: label,
          category_id: categoryId,
          isDone: isDone,
        };
        this.todos.push(newTodo);
      }
    },
    onSelectCategory(categoryId) {
      this.onSelectedCategId = categoryId;
    },
    getCategoryId() {
      return this.editedTask ? this.editedTask.category_id : 0;
    },
    onDelete(item) {
      let index = this.todos.findIndex(function(todo) {
        return todo.id == item.id;
      });
      this.todos.splice(index, 1);
      // console.log(this.todos[1])
    },

    onEdit(item) {
      let index = this.todos.findIndex(function(todo) {
        return todo.id == item.id;
      });
      console.log("Меняем: " + this.todos[index].label);
      document.querySelectorAll(".edit-field")[index].type = "text";
      document.querySelectorAll(".edit-done")[index].style.display =
        "inline-block";
      document.querySelectorAll(".edit-cancel")[index].style.display =
        "inline-block";
    },
    onEditDone(item, editedTask) {
      if (editedTask) {
        let index = this.todos.findIndex(function(todo) {
          return todo.id == item.id;
        });
        this.todos[index].label = editedTask;

        document.querySelectorAll(".edit-field")[index].type = "hidden";
        document.querySelectorAll(".edit-done")[index].style.display = "none";
        document.querySelectorAll(".edit-cancel")[index].style.display = "none";
      }
    },
    onEditCancel(item) {
      let index = this.todos.findIndex(function(todo) {
        return todo.id == item.id;
      });
      editedTask = "";
      document.querySelectorAll(".edit-field")[index].type = "hidden";
      document.querySelectorAll(".edit-field")[index].value = "";
      document.querySelectorAll(".edit-done")[index].style.display = "none";
      document.querySelectorAll(".edit-cancel")[index].style.display = "none";
    },
    onTaskDone(item) {
      if (item.isDone === false) {
        item.isDone = true;
      } else if (item.isDone === true) {
        item.isDone = false;
      }
    },

    onPostAdd(postTitle, postText, postDate) {
      if (postText && postTitle) {
        const newPost = {
          id_post: 0,
          title: postTitle,
          text: postText,
          tag_id: 0,
          year: postDate.getFullYear(),
          month: postDate.getMonth(),
          day: postDate.getDate(),
          time_hour: postDate.getHours(),
          time_min: postDate.getMinutes(),
        };
        this.posts.push(newPost);
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Pangolin&family=Seymour+One&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Balsamiq+Sans&display=swap");
* {
  scrollbar-color: #000;
}

::-webkit-scrollbar {
  width: 13px; /* ширина для вертикального скролла */
  height: 8px; /* высота для горизонтального скролла */
  background-color: #7b79fd;
}

/* ползунок скроллбара */
::-webkit-scrollbar-thumb {
  background-color: #beb3ff;
  border-radius: 9em;
}

::-webkit-scrollbar-thumb:hover {
  background-color: #d1b4e9;
}

/* Стрелки */

#app {
  width: 100%;
}
body {
  margin: 0;
  background: url(https://lookw.ru/8/828/1476173404-1-2.jpg);
  background-size: 130vw auto;
  background-repeat: no-repeat;
  background-attachment: fixed;

  font-family: "Pangolin", cursive;
  color: #111111;
  font-size: 36px;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin-bottom: 0;
  font-family: "Balsamiq Sans", cursive;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;

  padding-inline: 10%;
  width: 50vw;
  height: 100vh;
  overflow: auto;
  background: #ddd7ffdb;
}

.category-selector {
  display: inline-block;
}

li,
input,
button,
select,
option {
  padding: 5px;
  margin: 5px;
  background: #dad7ffdb;
  border: none;
  list-style: none;
}

select {
  display: inline-block;
}

ul {
  padding: 0;
}

button,
input,
select {
  border-radius: 15px;
  border: 5px solid #ffa5d900;
}

button:focus,
input:focus,
select:focus {
  outline: none;
  border: 5px solid #c28cfcde;
}

li {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 10px;
}

li p {
  flex-grow: 1;
}

.done {
  text-decoration: line-through;
}
.donetask {
  transition: 1s;
  opacity: 0.1;
}

@media (max-width: 400px) {
  body {
    background-size: cover;
    background-position: center;
    font-size: 14px;
  }
  .container {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    padding-inline: 0;

    overflow: auto;
    background: #ddd7ffdb;
  }
  li {
    width: 80%;
  }
  li,
  input,
  button,
  select,
  option {
    padding: 0;
    margin: 0;
    background: #dad7ffdb;
    border: none;
    list-style: none;

    height: 60px;
    text-align: center;
  }
}

@media (min-width: 401px) and (max-width: 1025px) {
  body {
    background-size: cover;
    background-position: center;
    font-size: 18px;
  }
  .container {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;

    padding-inline: 0;

    overflow: auto;
    background: #ddd7ffdb;
  }
  li {
    width: 80%;
  }
  li,
  input,
  button,
  select,
  option {
    padding: 5;
    margin: 7;
    background: #dad7ffdb;
    border: none;
    list-style: none;

    height: 60px;
    text-align: center;
  }
}

/*# sourceMappingURL=style.css.map */
</style>
