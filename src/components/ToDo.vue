<template>
  <li class="card" draggable="true">
    <div class="cb-container">
        <input 
            type="checkbox" 
            :checked="todo.isComplete ? true : null" 
            class="cb-input" 
        />
        <span class="check"></span>
    </div>
    <p class="item">{{todo.title}}</p>
    <!-- <p class="item"><span v-text="todo.title"></span></p> -->
    <button class="clear">Clear it</button>
  </li>
</template>

<script>
export default {
    props: {
        todo: Object,
    }
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