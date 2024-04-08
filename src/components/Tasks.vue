<template>
    <div class="ui container">
      <h2 class="header">Task List </h2>
      <div class="ui  grid">
       <div class="sixteen column wide rounded border-secondary border-large">
        <table class="ui table full-width striped "> <!-- Added full-width class here -->
          <thead class="full-width row">
            <th>Task ID</th>
            <th colspan="2">Task</th>
            <th>Category</th>
            <th>Date</th>
            <th>Status</th>
            <th>Actions</th>
          </thead>
          <tbody>
            <tr v-for="task in props.tasks" :key="task.id">
              <td>{{ task.id }}</td>
              <td colspan="2">{{ task.task }}</td>
              <td>{{ task.category }}</td>
              <td>{{ task.dateToDo }}</td>
              <td>
                    <i v-if="task.status === 'Completed'" class="green check icon"></i>
                    <i v-else class="gray clock icon"></i>
                   
                </td>
              <td>
                <div style="display: flex;">
                  <button class="ui button" @click="OpenModal(task)"><i class="edit teal icon"></i></button>

                   <!-- Modal -->
                

                  <button @click="$emit('delete-task', task.id); " class="ui button"><i class="trash red icon"></i></button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
       </div>
      </div>

      <div class="ui modal" id="example-modal">
        <i class="close icon"></i>
        <div class="header">
            Modal Header
        </div>
        <div class="content">
            <form @submit.prevent="editTask()" class="ui form  p-3 rounded-large shadow-secondary">
            <div class="ui message info" v-if="showMessage">
                <div>
                    {{ message }}
                </div>
                <i @click="hideMessageBox" class="close icon"></i>
            </div>
          <div class="field" >
            <label> Task</label>
            <input v-model="tsk" type="text" placeholder="e.g clean a compound">

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
                <div>
                    <div class="field"  style="display: flex;">
                    <input :checked="!completed" name="status" v-model="status" value="Pending"   type="radio"/>
                    <label style="margin-left: 20px;">Pending</label>
                </div>
                    <div class="field" style="display: flex;">
                        <input :checked="completed"  v-model="status" value="Completed" type="radio" name="status">
                        <label style="margin-left: 20px;">Completed</label>
                    </div>
                </div>
                    <div class="actions  rounded-secondary m-2 p-1">
                        <button type="submit" class="ui teal button">Edit Task</button>

                        <div class="ui cancel button">Cancel</div>
        </div>
            </form>
        </div>
  
        </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps,ref ,defineEmits} from 'vue';
  


  const props = defineProps({
    tasks: {
      type: Array,
      required: true
    }
  });



const tsk=ref("")
const dateToDo=ref("")
const status=ref("")
const category=ref("")

const task_id=ref("")

const completed=ref(false)

const showMessage=ref(false)
  const DeleteTask=(id)=>{
    console.log(id)
    $emit("delete-task", id); 
    }

    const OpenModal=(task)=>{
       task_id.value=task.id
       tsk.value=task.task
       category.value=task.category
       dateToDo.value=task.dateToDo
       completed.value = task.status === "Completed";
       console.log(task)

      $("#example-modal").modal("show")
    }
   const emit = defineEmits(['inFocus', 'edit-task'])
   const editTask=()=>{
       const editedTask={
        id:task_id.value,
        category:category.value,
        task:tsk.value,
        status:status.value,
        dateToDo:dateToDo.value
       }


       emit("edit-task",editedTask,task_id.value)
       $("#example-modal").modal("hide")
   }


  </script>
  