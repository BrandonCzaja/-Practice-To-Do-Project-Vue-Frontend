<template>
  <div></div>
</template>

<script>
export default {
  // Name of the Component
  name: "App",
  // data property has a function that returns an object with app data
  data: function() {
    return {
      todos: [],
      baseUrl: "http://localhost:3000/todos"
    };
  },
  // methods is an object of functions
  methods: {
    getTodos: async function() {
      const response = await fetch(this.baseUrl);
      const data = await response.json();
      this.todos = data;
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
