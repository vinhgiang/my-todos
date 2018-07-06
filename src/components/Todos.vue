<template>
  <section class="todoapp">
      <header class="header">
        <h1>{{ title }}</h1>
        <input v-model="taskName" @keypress.enter="AddTodoItem()" autofocus="autofocus" autocomplete="off" placeholder="What needs to be done?" class="new-todo">
      </header>
      <section class="main">
        <input @click="ToggleAll()" v-model="toggleAll" type="checkbox" class="toggle-all">
        <ul class="todo-list">
            <li v-for="(item, index) in getItemList" :key="index" :class="item.completed ? 'todo completed' : 'todo' ">
                <div class="view">
                    <input v-model="item.completed" type="checkbox" :checked="item.completed" class="toggle">
                    <label>{{ item.name }}</label>
                    <button @click="RemoveTodoItem(item.name)" class="destroy"></button>
                </div>
            </li>
        </ul>
    </section>
    <footer class="footer">
      <span class="todo-count">
          <strong>{{ getTotalIncompleteItem }}</strong> item left
      </span>
      <ul class="filters">
          <li><a href="" @click="showAll($event)" :class="this.status == 'all' ? 'selected' : ''">All</a></li>
          <li><a href="" @click="showActive($event)" :class="this.status == 'active' ? 'selected' : ''">Active</a></li>
          <li><a href="" @click="ShowCompleted($event)" :class="this.status == 'completed' ? 'selected' : ''">Completed</a></li>
      </ul>
      <button @click="ClearCompleted()" class="clear-completed">
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
      title: 'My Todos',
      taskName: '',
      toggleAll: false,
      todoList: [],
      status: 'all'
    }
  },
  methods: {
    AddTodoItem() {
      if (this.todoList.findIndex(e => e.name === this.taskName) >= 0) {
        return alert(this.taskName + ' is existing in your list.');
      }
      this.todoList.push({
        name: this.taskName,
        completed: false
      });

      this.taskName = '';
    },

    RemoveTodoItem(taskName) {
      let index = this.todoList.findIndex(e => e.name === taskName)
      this.todoList.splice(index, 1);
    },

    ToggleAll() {
      this.todoList.map(e => e.completed = ! this.toggleAll);
    },

    ClearCompleted() {
      this.todoList = this.todoList.filter(e => e.completed !== true);
    },

    ShowCompleted(e) {
      this.status = 'completed';
      e.preventDefault();
    },

    showActive(e) {
      this.status = 'active';
      e.preventDefault();
    },

    showAll(e) {
      this.status = 'all'
      e.preventDefault();
    }
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
    },

    getItemList() {
      let items = this.todoList;
      if(this.status === 'active') {
        return items.filter(e => e.completed === false)
      } else if (this.status === 'completed') {
        return items.filter(e => e.completed === true)
      }
      return items;
    }
  }
}
</script>

<style>
@import './../assets/css/base.css';
@import './../assets/css/index.css';
</style>
