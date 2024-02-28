
<template>
  <div class="container">
    <h1>Todos</h1>
    <br>

    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" @click="doneTask(todo)" :class="{ done: todo.done }">
        {{ todo.text }}
        <button class="remove-btn" @click="removeTask(todo)">X</button>
      </li>
    </ul>

    <div class="input-container">
      <input type="text" v-model="todoText" placeholder="Add new task" @keyup.enter="addTask">
      <button class="add-btn" @click="addTask">Add</button>
    </div>
  </div>
</template>

<script >
let id = 1;

export default {
  data() {
    return {
      todoText: null,
      todos: [
        {
          id: id++,
          text: 'Learn Vue',
          done: false,
        },

      ],
    };
  },
  methods: {
    addTask() {
      if (this.todoText) {
        const todo = {
          id: id++,
          text: this.todoText,
          done: false,
        }
        this.todos.push(todo)
        this.todoText = null;
        this.saveTodos();
      }
    },
    doneTask(todo) {
      todo.done = !todo.done;
      this.saveTodos();
    },
    removeTask(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
      this.saveTodos();
    },
    saveTodos() {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    }
  },
  created() {
    const storedTodos = localStorage.getItem('todos')
    this.todos = storedTodos ? JSON.parse(storedTodos) : []


  }
}


</script>
<style scoped>
.container {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  transform: translateY(15vh);



}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-list li {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  cursor: pointer;
}

.todo-list li.done {
  text-decoration: line-through;
}



.input-container {
  margin-top: 20px;
}

.input-container input {
  width: 60%;
  border: 1px solid #ccc;
  border-radius: 5px;
}



button {
  background-color: #00000044;
  border: none;
  color: white;
  text-align: center;
  text-decoration: none;
  padding: 5px 10px;
  float: right;


}

.done {
  text-decoration: line-through;
}
</style>
