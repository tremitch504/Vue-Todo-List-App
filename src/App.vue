<script setup>
import {ref, onMounted, computed, watch} from 'vue';

const ourTodos = ref([]); //store our todo data 

const name = ref(''); //name of the user when type in

const inputFieldData = ref(''); //what the user will type for a todo

const inputCategory = ref(null);

const todosOrder =  computed(() => ourTodos.value.sort((a,b) => {
  return b.createdAt - a.createdAt;
}))

//When a user types in their name, when the page refreshes, it disappears.
//We need to avoid that from happening.
watch(name, (newInputVal) => {
  //Here we can set name inside localStorage and assign it to whatever input value is passed in.
  localStorage.setItem('name', newInputVal)
})

//To avoid our data to disappear whenever we refresh the page, make sure to put it in onMounted
onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  ourTodos.value = JSON.parse(localStorage.getItem('ourTodos')) || []
})

const addTodo = () => {
//check if the given input data is an empty string or just null

if(inputFieldData.value.trim() === '' || inputFieldData.value === null){
  return; 
}

//add the given values in our array
//remember this is going into our todos array and not our local storage
ourTodos.value.push({
  todo: inputFieldData.value,
  category: inputCategory.value,
  done: false, //the status of the todo, if user have done it or not.
  createdAt: new Date().getTime()  //get our date of when the todo content is added
})

inputFieldData.value = ''  
inputCategory.value = null
}

const deleteTodo = (todo) => {
//we can filter out the array and only return the todo that equal with the given todo that needs to be removed
ourTodos.value = ourTodos.value.filter(data => data !== todo);

} 


watch(ourTodos, (newInputVal) => {
  localStorage.setItem('ourTodos', JSON.stringify(newInputVal));
  console.log(ourTodos);
}, { deep: true }) 

</script>

<!-- ref: is basically how we will handle our state -->
<!-- onMounted: is how we will render the components to our page -->
<!-- computed: Have things in order. all the data from our array in order -->
<!-- Our data will be organized by date -->
<!-- watch: will watch for any changes in our state or ref, and will update our local storage  -->

<template>
<main class="app">

  <section class="welcome">
    <h2 class="title">
      Hey! and Welcome, <input type="text" placeholder="Place Name here" v-model="name" />
    </h2>
  </section>


  <section class="create-todo">
    <h3>
      MAKE A TODO
    </h3>
   
    <!-- This form is using a submit handler with a prevent modifier  -->
    <!-- basically when using a button or input submit, we don't have to use a event handler to call the function inside. -->
    <form @submit.prevent="addTodo">
        <h4>What do you want to do?</h4>
        <input class="formInput" type="text" placeholder="ex. Go on a cruise" v-model="inputFieldData">
 
        <h4>Choose a category</h4>

        <div class="options">
          <label>
            <input type="radio" name="category" id="" value="Professional Business Work" v-model="inputCategory" />
            <span class="bubble professional"></span>
            <div>Career Work</div>
          </label>
  
          <label>
            <input type="radio" name="category" id="" value="Personal" v-model="inputCategory" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
         
        <!-- created a button for the form to submit and add the todo to our list, call/activate the addTodo function we made to add todos -->
        <button class="todo-btn" type="submit">Add TODO</button>
    </form>
  </section>

  <section class="list">
<h3>LIST OF TODOS</h3>

<div class="todos-list">
<div v-for="todo in todosOrder" :class="`todo-item ${todo.done && 'done'}`">
<label>
    <input type="checkbox" v-model="todo.done" />
    <span :class="`bubble ${todo.category == 'Professional Business Work' ? 'Professional Business Work' : 'personal'}`"></span>
  </label>

  <div class="todo-value">
    <input type="text" v-model="todo.todo" />
  </div>

  <div class="activity">
    <button class="delete" @click="deleteTodo(todo)">Delete </button>
  </div>
</div>

</div>
  </section>

</main>

</template>


