<template>
  <header>
    <h1 style="text-align: center">{{ title }}</h1>
  </header>

<main>
    <section id="todoList" v-if="todoList.length">
      <h2 id="todoListTitle">{{ todoTitle }}</h2>
      <div v-for="todos in todoList" :key="todos.id" :id="'todo-' + todos.id" class="todo-item">
        <input type="checkbox" v-model="todos.finished">
        <p :class="todos.finished ? 'finished' : ''">{{ todos.title }}</p>
        <button :class="'delete-todo ' + todos.id" @click="removeTodoItem(todos.id)">X</button>
      </div>
    </section>

    <section id="addToDoList">
      <input type="text" v-model="todoItem.title" @keydown.enter="addToDoItem(todoItem)" placeholder="Add a new item...">
      <button id="addToDo" @click="addToDoItem(todoItem)">submit</button>
      <p id="errorMessage" v-if="errorMessage.show">{{ errorMessage.title}} </p>
    </section>

</main>



</template>

<script setup lang="ts">
  import { ref } from 'vue'

  const title = ref('Welcome to my Vue components!');
  const errorMessage = ref({title: 'Enter a task first!', show: false});
  const todoTitle = ref('My To-Do list:')
  const todoList = ref([{title: 'Water plants', finished: false, id: 1}, {title: 'Vacuum', finished: false, id: 2}, {title: 'Fold laundry', finished: false, id: 3}]);
  const todoItem = ref({title: '', finished: false, id: null})

  function removeTodoItem(id) {
    // console.log(id);
    todoList.value = todoList.value.filter(todo => todo.id !== id);
  }

  function addToDoItem(item) {
    if (item.title.length < 1) {
      errorMessage.value.show = true;
      return
    }
    errorMessage.value.show = false;
    todoList.value.push({...item, id: todoList.value.length + 1});
    todoItem.value.title = '';
  }
</script>

<style scoped>
#todoList {
  font-size: 24px;
  padding-top: 24px;
  #todoListTitle {
    width: 100%;
    text-align: center;
  }
  .todo-item {
    padding-left: 24px;
    display: flex;
    min-width: 250px;
    justify-content: space-between;
    align-items: center;
    gap: 16px;
    .finished {
      text-decoration: line-through;
    }
    &:hover .delete-todo {
      opacity: 1;
    }
  }
  .delete-todo {
    color: #b10303;
    font-weight: bold;
    max-height: 24px;
    scale: 0.8;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.3s;
  }
}

#errorMessage {
  color: red;
  font-weight: bolder;
  padding-left: 5%;
}

#addToDoList {
  padding-top: 24px;
  #addToDo {
    margin-left: 16px;
  }
}

</style>
