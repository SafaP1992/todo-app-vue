<template>
  <li class="card" draggable="true">
    <div class="cb-container">
        <input 
            type="checkbox" 
            :checked="todo.isComplete ? true : null" 
            class="cb-input"
            @click="changeStatus"
        />
        <span class="check"></span>
    </div>
    <p class="item">
        <del v-if="todo.isComplete==true">{{todo.title}}</del>
        <span v-else v-text="todo.title"></span>
    </p>
    <!-- <p class="item"><span v-text="todo.title"></span></p> -->
    <button @click="deleteToDo" class="clear">X</button>
  </li>
</template>

<script>
export default {
    props: {
        todo: Object,
    },
    methods: {
        deleteToDo(){
            if(confirm('Are you sure to delete it?')){
                this.$emit('Deleted', this.todo.id);
            }
        },
        changeStatus(){
            this.$emit('changeStatus',this.todo.id, !this.todo.isComplete);
        },
    },
}
</script>

<style>
.card {
    display: flex;
    gap: 1em;
    list-style-type: none;
    background-color: rgb(215, 205, 224);
    padding: 7px;
    border-radius: 6px;
    align-content: center;
    align-items: center;
    text-align:center;
    margin: 5px auto;
}
button {
    padding: 3px;
}

</style>