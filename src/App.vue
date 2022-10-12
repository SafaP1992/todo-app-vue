<template>
 
  <app-header></app-header>

    <main>
        <div class="container">

          <add-to-do></add-to-do>

            <section>
                <ul class="todo">
                    <to-do v-for="(item, index) in todos" :key="index" :todo="item"></to-do>
                </ul>
            </section>

        </div>
    </main>

    <app-footer></app-footer>
    
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
            { title: "Tutorial PHP", isComplete: false},
            { title: "Tutorial React", isComplete: true},
            { title: "Tutorial Vue JS", isComplete: false}
        ]
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
</style>
