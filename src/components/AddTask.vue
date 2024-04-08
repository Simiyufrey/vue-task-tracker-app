<template>

<div class="ui segment" style=" display: flex; justify-content: center;">
      <div style="width:500px;">
        <form @submit.prevent="addTask" class="ui form  p-3 rounded-large shadow-secondary">
            <div class="ui message info" v-if="showMessage">
                <div>
                    {{ message }}
                </div>
                <i @click="hideMessageBox" class="close icon"></i>
            </div>
          <div class="field" >
            <label> Task</label>
            <input v-model="task" type="text" placeholder="e.g clean a compound">

          
          </div>


          <div class="field" >
            <label> Category</label>
            <select v-model="category" name="" id="">
              <option disabled value="Manual Task">Select Task</option>
              <option value="Manual Task">Manual Task</option>
              <option value="Automated Task">Automated Task</option>
            </select>
          </div>
    
          <div class="field" >
            <label> Date</label>
            <input v-model="dateToDo" type="date" placeholder="">
           
          </div>
          <button type="submit" class="ui green button">Add Task</button>
        </form>
      </div>
  </div>


</template>


<script setup>
 import {ref,onMounted, watch,defineProps} from 'vue';
 
 const props=defineProps({
    tasks:{
      type:Array,
      required:true
    }

  })
 const task=ref("")

 const message=ref("")

 const category=ref("")

 const showMessage=ref(false)

 const dateToDo=ref("")


 console.log(props.tasks)
  onMounted(()=>{
 
  })
 const addTask=()=>{
    if(task.value ===""  || category.value ===""  || dateToDo ===""){
        showMessage.value=true

         message.value="All fields are required"

        return
    }

    const newTask={
        id: Math.floor(Math.random() * 10000),
        task: task.value,
        category:category.value,
        dateToDo:dateToDo.value,
        status:"Pending"

    }

    props.tasks.push(newTask)

    console.log(props.tasks)
    
 }

 const hideMessageBox=()=>{
    showMessage.value=false
 }

</script>



<style scoped>


</style>