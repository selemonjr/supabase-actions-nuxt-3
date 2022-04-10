<script setup lang="ts">
import { Task } from '~/types/tasks';
import TrashIcon from "../components/TrashIcon.vue";
definePageMeta({
  middleware: 'auth'
})
const client = useSupabaseClient()
const user = useSupabaseUser()
const loading = ref(null)
const newTask = ref('')
const { data: tasks } = await useAsyncData('tasks', async () => {
  return await client.from<Task>('tasks').select('id, todo, completed').eq('user', user.value.id).order('created_at')
}, {
  transform: result => result.body
})
async function addTask () {
  if (newTask.value.trim().length === 0) {
    return
  }
  loading.value = true
  const { data } = await client.from<Task>('tasks').insert({
    user: user.value.id,
    todo: newTask.value,
    completed: false
  })
  tasks.value.push(data[0])
  newTask.value = ''
  loading.value = false
}
// const completeTask = async (task: Task) => {
//   await client.from<Task>('tasks').update({ completed: task.completed }).match({ id: task.id })
// }
const deleteTask = async (task: Task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
  await client.from<Task>('tasks').delete().match({ id: task.id })
}
</script>
<template>
    <div class="w-full h-96 -mt-40">
    <div class="grid place-items-center py-5">
        <h2 class="text-2xl text-white font-extrabold">Todo List</h2>
    </div>
    <div class="grid place-items-center">
       <form @submit.prevent="addTask">
           <div class="flex justify-center items-center">
            <input type="text" v-model="newTask" placeholder="Add Task" class="md:w-[500px] w-[200px] text-sm md:text-xl ml-5 md:m-0 rounded-md shadow-md py-2 md:py-2 px-2 focus:outline-none focus:ring-4 font-bold focus:ring-green-600">
        <button type="submit" class="py-2 px-2 w-20 text-sm md:text-lg md:w-28 rounded-md text-white font-bold bg-green-500 ml-4 md:ml-5 focus:ring-4 ring-offset-1 hover:bg-green-600 focus:ring-green-600">{{loading ? "Loading..." : "Add Task"}}</button>
       </div>
       </form>
    </div>

    <div class="gridBox">
      <div class=" w-[280px] md:w-[640px] mt-5 py-3 min-h-[96] bg-gray-200 rounded-md shadow-md">
        <div>
          <div v-if="tasks.length === 0" class="gridBox">
          <h2 class="text-black font-bold text-sm md:text-2xl">You have no tasks</h2>
          </div>
            <ul v-else v-for="task in tasks" :key="task.id" class="flex justify-between items-center ">
            <div>
              <li class="py-3 px-5 font-bold text-black text-sm md:text-xl" :class="[task.completed ? 'line-through text-gray-500' : 'text-white']">{{task.todo}}</li>
            </div>
            <div class="flex justify-center items-center">
            <!-- <div @click="completeTask(task)">
              <input type="checkbox"  class="text-green-500 focus:ring-green-500 w-6 h-6 rounded-sm mr-2" v-model="task.completed">
            </div> -->
              <div>
                <TrashIcon @click="deleteTask(task)"/>
              </div>
            </div>
        </ul>
        </div>
      </div>
    </div>
    </div>
</template>