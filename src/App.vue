<script setup>
import {ref, onMounted, computed, watch} from 'vue';

const ourTodos = ref([]);

const name = ref('');

const inputFieldData = ref('');

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

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

const addTodo = () => {
  
}
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

    <form @submit.prevent="addTodo">
        <h4>What do you want to do?</h4>
        <input class="formInput" type="text" placeholder="ex. Go on a cruise" v-model="inputFieldData">
 
        <h4>Choose a category</h4>

        <div class="options">
          <label for="">
            <input type="radio" name="category" id="" value="Professional Business Work" v-model="inputCategory" />
            <span class="bubble professional"></span>
            <div>Career Work</div>
          </label>
  
          <label for="">
            <input type="radio" name="category" id="" value="Personal" v-model="inputCategory" />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>
         
        <button class="todo-btn" type="submit">Add TODO</button>
        <!-- <input type="submit" value="Add todo" /> -->
    </form>
  </section>
</main>

</template>


