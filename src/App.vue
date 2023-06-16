<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'

import ChildComp from './components/ChildComp.vue'
</script>

<!-- <template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it! OKE" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template> -->

<!-- CONTOH SCRIPT 1 -->
<script>
// give each todo a unique id
let id = 0
let id2 = 0
export default {
  // mounted() {
  //   this.$refs.pElementRef.textContent = 'mounted aku!'
  // },
  mounted() {
    this.fetchData()
  },
  watch: {
    todoId() {
      this.fetchData()
    }
  },
  data() {
    return {
      titleClass: 'title',
      count: 0,
      text: '',
      awesome: true,
      newTodo: '',
      todos: [
        { id: id++, text: 'Learn HTML' },
        { id: id++, text: 'Learn JavaScript' },
        { id: id++, text: 'Learn Vue' }
      ],
      newTodo2: '',
      hideCompleted: false,
      todos2: [
        { id2: id2++, text: 'Learn HTML', done: true },
        { id2: id2++, text: 'Learn JavaScript', done: true },
        { id2: id2++, text: 'Learn Vue', done: false }
      ],
      todoId: 1,
      todoData: null,
      greeting: 'Hello from parent1',
      childMsg: 'No child msg yet'

    }
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos2.filter((t) => !t.done)
        : this.todos2
    }
  },
  methods: {
    increment() {
      this.count++
    },
    toggle() {
      this.awesome = !this.awesome
    },
    addTodo() {
      this.todos.push({ id: id++, text: this.newTodo })
      this.newTodo = ''
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo)
    },

    addTodo2() {
      this.todos2.push({ id2: id2++, text: this.newTodo2, done: false })
      this.newTodo2 = ''
    },
    removeTodo2(todo2) {
      this.todos2 = this.todos2.filter((t) => t !== todo2)
    },

    async fetchData() {
      this.todoData = null
      const res = await fetch(
        `https://jsonplaceholder.typicode.com/todos/${this.todoId}`
        // `http://127.0.0.1:1007/api.organizationdevelopment.v1/newemployeeevaluation/2203051857`
      )
      this.todoData = await res.json()
    }

  },
  components: {
    ChildComp
  }
}
</script>

<template>
   <h1 :class="titleClass">Make me red</h1>
   <br>
  <button @click="increment">count is: {{ count }}</button>
  <br>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>

  <br>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  <br>

  <form @submit.prevent="addTodo">
    <input v-model="newTodo">
    <button>Add Todo</button>    
  </form>
  <ul>
    <li v-for="todo2 in todos" :key="todo2.id">
      {{ todo2.text }}
      <button @click="removeTodo(todo2)">X</button>
    </li>
  </ul> 
  <br>


  <form @submit.prevent="addTodo2">
    <input v-model="newTodo2">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo2 in filteredTodos" :key="todo2.id">
      <input type="checkbox" v-model="todo2.done">
      <span :class="{ done: todo2.done }">{{ todo2.text }}</span>
      <button @click="removeTodo(todo2)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>

  <br>
  <!-- <p ref="pElementRef">hello</p> -->

  <br>

  <div style="border: 1px solid;">
    
    <p>Todo id: {{ todoId }}</p>
    <button @click="todoId++">Fetch next todo</button>
    <p v-if="!todoData">Loading...</p>
    <pre v-else>{{ todoData }}</pre>
  </div>
  
  <!-- <div style="border: 1px solid;"> -->
    <!-- <ChildComp :msg="greeting" /> -->
  <!-- </div> -->

  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>

</template>




<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
