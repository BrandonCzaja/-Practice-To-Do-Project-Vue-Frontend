<template>
  <div>
    <h1>The Vue To Do App</h1>
    <hr />
    <h1>Add a To Do</h1>
    <form v-on:submit.prevent="createTodo">
      <input type="text" v-model="createTitle" />
      <input type="text" v-model="createBody" />
      <input type="submit" />
    </form>
    <hr />
    <h1>Edit a To Do</h1>
    <form v-on:submit.prevent="editTodo">
      <input type="text" v-model="editTitle" />
      <input type="text" v-model="editBody" />
      <input type="submit" />
    </form>
    <h3>To Dos</h3>
    <ul>
      <li v-for="todo of todos" v-bind:key="todo.id">
        **********************
        <h4>{{todo.title}}</h4>
        <h5>{{todo.body}}</h5>**********************
        <button v-on:click="()=> editSelect(todo)">Edit</button>
        <button v-on:click="()=> deleteTodo(todo)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  // Name of the Component
  name: "App",
  // data property has a function that returns an object with app data
  data: function() {
    return {
      todos: [],
      baseUrl: "http://localhost:3000/todos",
      createTitle: "",
      createBody: "",
      editTitle: "",
      editBody: "",
      editId: 0
    };
  },
  // methods is an object of functions
  methods: {
    getTodos: async function() {
      const response = await fetch(this.baseUrl);
      const data = await response.json();
      this.todos = data;
    },
    createTodo: async function() {
      await fetch(this.baseUrl, {
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          title: this.createTitle,
          body: this.createBody
        })
      });
      this.createTitle = "";
      this.createBody = "";
      this.getTodos();
    },
    editSelect: function(todo) {
      this.editTitle = todo.title;
      this.editBody = todo.body;
      this.editId = todo.id;
    },
    editTodo: async function() {
      await fetch(this.baseUrl + "/" + this.editId, {
        method: "put",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          title: this.editTitle,
          body: this.editBody
        })
      });
      this.editTitle = "";
      this.editBody = "";
      this.editId = 0;
      this.getTodos();
    },
    deleteTodo: async function(todo) {
      await fetch(this.baseUrl + "/" + todo.id, {
        method: "delete"
      });
      this.getTodos();
    }
  },
  // create runs after components is initially created, one of many lifecycle functions
  created: function() {
    this.getTodos();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
