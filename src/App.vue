<template>
  <div class="ui container fluid">
    <Header/>


  <!-- add task form -->

  <AddTask :tasks="tasks"/>
  
  <Tasks :tasks="tasks"  @delete-task="onDeleteTask"  @edit-task="editTask"/>
  </div>
</template>

<script setup>
import { ref , watch, onMounted} from 'vue';
import Header from './components/Header.vue';
import AddTask from './components/AddTask.vue';
import Tasks from './components/Tasks.vue';
import './main.css'
const showMessage = ref(true);

const tasks=ref([])

watch(tasks,newTasks=>{
  localStorage.setItem("tasks",JSON.stringify(newTasks))
},{deep:true})


onMounted(()=>{

  tasks.value=JSON.parse(localStorage.getItem("tasks"))  || []

  console.log(tasks.value)



})

const hideMessage = () => {
  showMessage.value = false;
};


const onDeleteTask=(id)=>{
  tasks.value=tasks.value.filter(task=> task.id !== id)
}

const editTask=(newTask,id)=>{



   tasks.value=tasks.value.map(task=>{
    if(task.id===id){
    
      return{...task,...newTask}
    }
    else{
      return task
    }
   })

}
</script>

<style scoped>
.custom-container {
  display: flex;
  justify-content: center;
}
</style>
