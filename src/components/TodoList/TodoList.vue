<template>
  <div>
    <input class="todo-input" type="text" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <div class="todo-item" v-for="(todo, index) in todosFiltered" :key="todo.id">
        <div class="todo-item--left">
          <input class="todo-item--left__checkbox" type="checkbox" placeholder="" v-model="todo.completed">
          <div v-if="!todo.editing" @click="editTodo(todo)" class="todo-item--left__label"
               :class="{completed : todo.completed}">{{todo.title}}
          </div>
          <input v-else @keyup.enter="doneEdit(todo)" @blur="doneEdit(todo)" @keyup.escape="cancelEdit(todo)"
                 class="todo-item--left__edit" type="text" placeholder="" v-model="todo.title" v-focus>
        </div>
        <div class="todo-item--right">
          <button class="todo-item--right__remove" @click="removeTodo(index)"
                  :class="{show: todo.completed}"> x
          </button>
        </div>
      </div>
    </transition-group>
    <div class="todo-remaining">
      <div>
        <label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">{{anyRemaining ? 'Check all' :
                                                                                       'Uncheck all'}}</label>
      </div>
      <div>{{remaining}} items left</div>
    </div>
    <div class="todo-filter">
      <div>
        <button class="btn" :class="{active: filter === 'all'}" @click="filter = 'all'">All</button>
        <button class="btn" :class="{active: filter === 'active'}" @click="filter = 'active'">Active</button>
        <button class="btn" :class="{active: filter === 'completed'}" @click="filter = 'completed'">Completed</button>
      </div>
      <transition name="fade">
        <button class="btn clear-btn" v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
      </transition>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'TodoList',
    data() {
      return {
        newTodo: '',
        beforeEdit: '',
        filter: 'all',
        todos: [
          {
            id: +new Date(),
            title: 'Learn Vue.js',
            completed: false,
            editing: false
          },
          {
            id: +new Date() + 1,
            title: 'Take over World',
            completed: false,
            editing: false
          }
        ]
      };
    },
    computed: {
      remaining() {
        return this.todos.filter(todo => !todo.completed).length;
      },
      anyRemaining() {
        return this.remaining !== 0;
      },
      todosFiltered() {
        if (this.filter === 'all') {
          return this.todos;
        } else if (this.filter === 'active') {
          return this.todos.filter(todo => !todo.completed);
        } else if (this.filter === 'completed') {
          return this.todos.filter(todo => todo.completed);
        }
        return this.todos;
      },
      showClearCompletedButton() {
        return this.todos.filter(todo => todo.completed).length > 0;
      }
    },
    directives: {
      focus: {
        inserted: function(el) {
          el.focus();
        }
      }
    },
    methods: {
      addTodo() {
        this.todos.push({
          id: +new Date(),
          title: this.newTodo,
          completed: false,
          editing: false
        });
        this.newTodo = '';
      },
      removeTodo(index) {
        this.todos.splice(index, 1);
      },
      editTodo(todo) {
        this.beforeEdit = todo.title;
        todo.editing = true;
      },
      doneEdit(todo) {
        if (todo.title === '') {
          todo.title = this.beforeEdit;
        }
        todo.editing = false;
      },
      cancelEdit(todo) {
        todo.title = this.beforeEdit;
        todo.editing = false;
      },
      checkAllTodos() {
        this.todos.forEach(todo => todo.completed = event.target.checked);
      },
      clearCompleted() {
        return this.todos = this.todos.filter(todo => !todo.completed);
      }
    }
  };
</script>

<style lang="scss">
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

  .todo-input {
    width: 100%;
    padding: 10px 0;
    font-size: 16px;
    margin-bottom: 10px;
    border: none;
    border-bottom: 1px solid #35495e;
    &:focus {
      outline: none
    }
  }

  .todo-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 0 10px 0;
    margin-bottom: 10px;
    border-bottom: 1px solid #41b883;
    height: 40px;
    &--left {
      display: flex;
      align-items: center;
      &__label {
        word-break: break-all;
        line-height: 1.5;
        transition: all .3s;
      }
      &__edit {
        font-size: 16px;
        border: none;
        color: #41b883;
      }
      .completed {
        text-decoration: line-through;
        color: #ccc;
      }
    }
    &--right {
      &__remove {
        color: red;
        font-size: 18px;
        background-color: transparent;
        cursor: pointer;
        border: none;
        opacity: 0;
        visibility: hidden;
        transition: all .3s;
      }
      .show {
        opacity: 1;
        visibility: visible;
      }
    }
  }

  .todo-remaining {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 0 10px 0;
    margin-bottom: 10px;
    height: 40px;
  }

  .todo-filter {
    display: flex;
    justify-content: space-between;
    align-items: center;
    > div {
      .btn {
        &:last-child {
          margin-right: 0;
        }
      }
    }
    .clear-btn {
      border-color: #fd1a2d;
      margin-right: 0;
      width: 140px;
      color: #fd1a2d;
      &:hover {
        background: #fd1a2d;
        color: #fff;
      }
    }
    .active {
      background-color: #41b883;
      color: #fff;
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }

  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
</style>
