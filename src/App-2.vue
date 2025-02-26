<!-- options APi -->
<script>
  export default {
      data(){
        return{
          name: `Vitor Torquato`,
          status:'pending',
          tasks: ['task one' , 'task two' , 'task three'],
          link: 'https://google.com'
        }
      },
      methods:{
        toggleStatus(){
          if(this.status === 'active'){
            this.status = 'pending'
          }else if(this.status === 'pending'){
            this.status = 'inactive'
          }else{
            this.status = 'active'
          }
        }
      }
  }
</script>

<template>
  <h1>Hello {{ name }}. You'll get this vue job</h1>
  <p v-if="status === 'active'">User is active</p> <!-- condicionals-->
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>Use is inactive</p>


  <h3>tasks:</h3>

  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li> <!--loop-->
  </ul>

  <a :href="link" target="_blank">click for google</a> <!--v-bind posso usar somente :-->

  <br/>
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>
</template>


<!-- composition APi -->
<script setup>

      import { onMounted, ref } from 'vue';

      const  name = ref('Vitor')
      const status = ref('active');
      const tasks = ref(['task one' , 'task two' , 'task three'])
      const newTask = ref(' ');
    
      const toggleStatus = () => {
        if(status.value === 'active'){
            status.value = 'pending'
          }else if(status.value === 'pending'){
            status.value = 'inactive'
          }else{
            status.value = 'active'
          }
      }

      const handleAddTask = () => {
          if(newTask.value.trim() !== ''){
            tasks.value.push(newTask.value);
            newTask.value = '';
          }
      }

      const handleDeleteTask = (index) => {
          tasks.value.splice(index , 1)
      }


      onMounted( async () => {
        try{
          const response = await fetch('https://jsonplaceholder.typicode.com/todos')
          const data = await response.json();
          tasks.value = data.map((task) => task.title)
        }catch(error){
          console.log(`Error fetching data`)

        }
      });
  
</script>

<template>
  <h1>Hello {{ name }}. You'll get this vue job</h1>
  <p v-if="status === 'active'">User is active</p> <!-- condicionals-->
  <p v-else-if="status === 'pending'">User is pending</p>
  <p v-else>Use is inactive</p>

  <form @submit.prevent="handleAddTask">
      <label for="newTaks">Add task</label>
      <input type="text" name="newTask" id="newTask" v-model="newTask">
      <button type="submit">submit</button>
  </form>

  <h3>tasks:</h3>

  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="handleDeleteTask(index)">x</button>
    </li> <!--loop-->
  </ul>




  <br/>
  <!-- <button v-on:click="toggleStatus">Change Status</button> -->
  <button @click="toggleStatus">Change Status</button>
</template>


