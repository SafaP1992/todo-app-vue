<template>
 
  <app-header></app-header>

    <main>
        <div class="container">
            <button id="singup">Sign Up</button>
            <div class="form">
      
                <ul class="tab-group">
                    <li class="tab active"><a id="tab">Sign Up</a></li>
                    <li class="tab"><a>Log In</a></li>
                </ul>
                
                <div class="tab-content">
                    <register-app></register-app>
                    <login-app></login-app>
                </div><!-- tab-content -->
            </div> <!-- /form -->

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
import AddToDo from './components/todo/AddToDo.vue'
import ToDo from './components/todo/ToDo.vue'
import LoginApp from './components/form/LoginApp.vue'
import RegisterApp from './components/form/RegisterApp.vue'

export default {
  name: 'App',
  components: {
    AppHeader,
    AppFooter,
    AddToDo,
    ToDo,
    LoginApp,
    RegisterApp,
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
  /* text-align: center; */
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


/* Login/Register Form */

a {
  text-decoration: none;
  color: #1ab188;
  transition: .5s ease;
}
a:hover {
  color: #179b77;
}

.form {
  background: rgba(19, 35, 47, 0.9);
  padding: 40px;
  max-width: 600px;
  margin: 40px auto;
  border-radius: 4px;
  box-shadow: 0 4px 10px 4px rgba(19, 35, 47, 0.3);
}

.tab-group {
  list-style: none;
  padding: 0;
  margin: 0 0 40px 0;
}
.tab-group:after {
  content: "";
  display: table;
  clear: both;
}
.tab-group li a {
  display: block;
  text-decoration: none;
  padding: 15px;
  background: rgba(160, 179, 176, 0.25);
  color: #a0b3b0;
  font-size: 20px;
  float: left;
  width: 50%;
  text-align: center;
  cursor: pointer;
  transition: .5s ease;
}
.tab-group li a:hover {
  background: #179b77;
  color: #ffffff;
}
.tab-group .active a {
  background: #1ab188;
  color: #ffffff;
}

.tab-content > div:last-child {
  display: none;
}

h1 {
  text-align: center;
  color: #ffffff;
  font-weight: 300;
  margin: 0 0 40px;
}

label {
  /* position: absolute; */
  /* transform: translateY(6px); */
  /* left: 13px; */
  color: rgba(255, 255, 255, 0.5);
  transition: all 0.25s ease;
  -webkit-backface-visibility: hidden;
  /* pointer-events: none; */
  text-align: right;
  font-size: 16px;
}
label .req {
  margin: 2px;
  color: #1ab188;
}

label.active {
  transform: translateY(50px);
  left: 2px;
  font-size: 14px;
}
label.active .req {
  opacity: 0;
}

label.highlight {
  color: #ffffff;
}

input, textarea {
  font-size: 22px;
  display: block;
  width: 100%;
  height: 100%;
  padding: 5px 10px;
  background: none;
  background-image: none;
  border: 1px solid #a0b3b0;
  color: #ffffff;
  border-radius: 0;
  transition: border-color .25s ease, box-shadow .25s ease;
}
input:focus, textarea:focus {
  outline: 0;
  border-color: #1ab188;
}

textarea {
  border: 2px solid #a0b3b0;
  resize: vertical;
}

.field-wrap {
  position: relative;
  margin-bottom: 40px;
}

.top-row:after {
  content: "";
  display: table;
  clear: both;
}
.top-row > div {
  float: left;
  width: 48%;
  margin-right: 4%;
}
.top-row > div:last-child {
  margin: 0;
}

.button {
  border: 0;
  outline: none;
  border-radius: 0;
  padding: 15px 0;
  font-size: 2rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: .1em;
  background: #1ab188;
  color: #ffffff;
  transition: all 0.5s ease;
  -webkit-appearance: none;
}
.button:hover, .button:focus {
  background: #179b77;
}

.button-block {
  display: block;
  width: 100%;
}

.forgot {
  margin-top: -20px;
  text-align: right;
}

.error-alert {
  color: rgb(165, 165, 165);
}

</style>
