<template>
  <div id="app" class="container">
    <h1><span>Vue</span>ToDo</h1>
    <TodoList />
  </div>
</template>

<script>
  import TodoList from './components/TodoList/TodoList';

  export default {
    name: 'App',
    components: {
      TodoList
    }
  };
</script>

<style lang="scss">
  * {
    font-family: "Roboto", sans-serif;
    box-sizing: border-box;
    margin: 0;
    body {
      height: calc(100vh - 50px);
      background: #eee;
      margin-top: 50px;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      span {
        color: #41b883;
      }
    }
    input {
      background: transparent;
    }
    button, input {
      outline: none;
    }
    button, input[type="checkbox"], label {
      cursor: pointer;
    }
    input[type="checkbox"] {
      margin-right: 10px;
    }
    .btn {
      background-color: transparent;
      font-size: 16px;
      height: 40px;
      width: 110px;
      border: 1px solid #41b883;
      color: #41b883;
      margin-right: 10px;
      transition: all .3s;
      &:hover {
        background-color: #41b883;
        color: #fff;
      }
    }
    .container {
      max-width: 600px;
      margin: 0 auto;
    }
  }
</style>
