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
    <h3>To Dos</h3>
    <ul>
      <li v-for="todo of todos" v-bind:key="todo.id">
        **********************
        <h4>{{todo.title}}</h4>
        <h5>{{todo.body}}</h5>**********************
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
      createBody: ""
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
      (this.createTitle = ""), (this.createBody = ""), this.getTodos();
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
