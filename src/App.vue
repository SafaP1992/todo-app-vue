<template>
 
  <app-header></app-header>

    <main>
        <div class="container">


          <add-to-do v-on:AddNewToDo="AddToDo"></add-to-do>

            <div class="btn-delete-all">
                <button>Delete All</button>
            </div>
            <section>
                <ul class="todo">
                    <to-do 
                        v-for="item in todos" 
                        :key="item.id" 
                        :todo="item" 
                        @Deleted="DeletedTodo"
                        @changeStatus="changeTodoStaus"    
                    ></to-do>
                </ul>
            </section>

        </div>
    </main>

    <div class="container">
        <app-footer></app-footer>
    </div>
        
</template>

<script>
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import AddToDo from './components/AddToDo.vue'
import ToDo from './components/ToDo.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
    AddToDo,
    ToDo
  },
  data() {
    return {
        todos: [
            
        ]
    }
  },
  methods: {
    AddToDo(title) {
        const id = Math.random().toString(16).slice();
        const todo = { id, title, isComplete: false};
        this.todos.push(todo)
    },
    DeletedTodo(id){
        this.todos = this.todos.filter((f) => f.id !== id)
    },
    changeTodoStaus(id, newStatus){
        // console.log(id);
        // console.log(newStatus);
        var newTodos = [...this.todos];
        var selectedTodo = newTodos.find((f) => f.id === id);
        selectedTodo.isComplete = newStatus;
        this.todos = newTodos;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}

.todo {
    margin: 5px;
}

.btn-delete-all {
    text-align: right;
}
.btn-delete-all button{
    text-align: center;
    padding: 8px;
}
</style>
