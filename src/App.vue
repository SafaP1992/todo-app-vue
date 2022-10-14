<template>
 
  <app-header></app-header>

    <main>
        <div class="container">


          <add-to-do v-on:AddNewToDo="AddToDo"></add-to-do>

            <div class="btn-delete-all">
                <button @click="deleteCompleted">Delete Completed Task</button>
            </div>
            <section>
                <ul class="todo">
                    <to-do 
                        v-for="(item, i) in getTodo" 
                        :key="item.id" 
                        :todo="item"
                        @dragstart="dragStart(i)"
                        @Deleted="DeletedTodo"
                        @changeStatus="changeTodoStaus"
                        @dragover.prevent
                        @drop="drop(i)"
                    ></to-do>
                </ul>
            </section>
        </div>

        <div>
            <div class="filter">
                <button id="all" :class="{on:activeTab=='all'}" @click="changeTab('all')">All</button>
                <button id="active" :class="{on:activeTab=='active'}" @click="changeTab('active')">Active</button>
                <button id="completed" :class="{on:activeTab=='completed'}" @click="changeTab('completed')">Completed</button>
            </div>
            <span>Unfinished: {{getActiveTodoCount}}</span>
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
        todos: [],
        dragging: -1,
        activeTab: "all"
    }
  },
  computed: {
    getActiveTodoCount(){
        return this.todos.filter((f) => f.isComplete == false).length;
    },
    getTodo(){
        switch (this.activeTab) {
            case "all":
                return this.todos;
            case "active":
                return this.todos.filter((f) => f.isComplete == false);
            case "completed":
                return this.todos.filter((f) => f.isComplete == true);
            default:
                return this.todos;
        }
    }
  },
  methods: {
    AddToDo(title) {
        // Add new item
        const id = Math.random().toString(16).slice(2);
        const todo = { id, title, isComplete: false};
        this.todos.push(todo);
        this.$toast.success("Added Successfuly", {
            position: 'top-right'
        });
    },
    DeletedTodo(id){
        // Delete an item
        const todo = this.todos.find((f) => f.id == id);
        this.todos = this.todos.filter((f) => f.id !== id);
        this.$toast.error(`${todo.title} deleted successfuly`);
    },
    changeTodoStaus(id, newStatus){
        // Updating current status (checkbox)
        var newTodos = [...this.todos];
        var selectedTodo = newTodos.find((f) => f.id === id);
        selectedTodo.isComplete = newStatus;
        this.todos = newTodos;
    },
    deleteCompleted(){
        // Delete all
        if(confirm('Are you want to delete selected items?')){
            var newTodos = [...this.todos];
            newTodos = newTodos.filter(f => f.isComplete === false);
            this.todos = newTodos;
            this.$toast.success("Done", {
                position: 'top-right'
            });
        }
    },
    dragStart(index){
        // console.log(index);
        this.dragging = index;
    },
    drop(index){
        var newElement = this.todos.splice(this.dragging,1)[0];
        this.todos.splice(index,0,newElement);
    },
    changeTab(tab){
        this.activeTab = tab;
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

input {
    width: 50%;
    padding: 0.4em;
    border-radius: 0.6em;
    border: none;
}

button {
    width: 8em;
    padding: 0.4em;
    border: none;
    border-radius: 1em;
    cursor: pointer;
    margin: auto 0.3em;
}

.on {
    background-color: #5e6a77;
    color: #fff;
    font-weight: bold;
}

</style>
