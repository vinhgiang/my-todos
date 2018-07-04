<template>
  <section class="todoapp">
      <header class="header">
        <h1>{{ title }}</h1>
        <input v-model="taskName" @keypress.enter="AddTodoItem()" autofocus="autofocus" autocomplete="off" placeholder="What needs to be done?" class="new-todo">
      </header>
      <section class="main">
        <input type="checkbox" class="toggle-all">
        <ul class="todo-list">
            <li v-for="(item, index) in todoList" :key="index" :class="item.completed ? 'todo completed' : 'todo' ">
                <div class="view">
                    <input v-model="item.completed" type="checkbox" :checked="item.completed" class="toggle">
                    <label>{{ item.name }}</label>
                    <button @click="RemoveTodoItem(index)" class="destroy"></button>
                </div>
                <input type="text" class="edit">
            </li>
        </ul>
    </section>
    <footer class="footer">
      <span class="todo-count">
          <strong>{{ getTotalIncompleteItem }}</strong> item left
      </span>
      <ul class="filters">
          <li><a href="" class="selected">All</a></li>
          <li><a href="" class="">Active</a></li>
          <li><a href="" class="">Completed</a></li>
      </ul>
      <button class="clear-completed">
          Clear completed
      </button>
    </footer>
  </section>
</template>

<script>
export default {
  name: 'Todos',
  data () {
    return {
      taskName: '',
      title: 'My Todos',
      todoList: []
    }
  },
  methods: {
    AddTodoItem() {
      this.todoList.push({
        name: this.taskName,
        completed: false
      });

      this.taskName = '';
    },

    RemoveTodoItem(index) {
      this.todoList.splice(index, 1);
    },
  },
  computed: {
    getTotalIncompleteItem() {
      let totalIncompleteItem = 0;
      for(let i = 0; i < this.todoList.length; i++) {
        if( ! this.todoList[i].completed ) {
          totalIncompleteItem++;
        }
      }
      return totalIncompleteItem;
    }
  }
}
</script>

<style>
@import './../assets/css/base.css';
@import './../assets/css/index.css';
</style>
