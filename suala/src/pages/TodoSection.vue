<template>
  <q-page>
    <div class="row q-pa-sm bg-primary">
      <q-input
      @keyup.enter="addTask"
      class="col"
      bg-color="white"  
      v-model="newTask" 
      placeholder="Add Task" 
      square
      dense>
      <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list>
      <q-item 
      v-for="(list, index) in lists" 
      :key="list.title" 
      @click="list.completed = !list.completed"
      :class="{'done bg-blue-1': list.completed}"
      clickable 
      v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="list.completed" class="no-pointer-events" color="teal" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ list.title }}</q-item-label>
        </q-item-section>
        <q-item-section
        v-if="list.completed"
        side
        >
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!lists.length" class="no-tasks absolute-center">
      <q-icon
      name="check"
      size="102px"
      color="primary"
       />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data(){
  return {
    newTask: '',
    lists: [
      // {
      //   title: "Clean the Car",
      //   completed: false
      // },
      // {
      //   title: "Code",
      //   completed: false
      // },
      // {
      //   title: "Gym",
      //   completed: false
      // }
    ]
  }
},
methods: {
  deleteTask(index){
      this.$q.dialog({
        dark: true,
        title: 'Confirm',
        message: 'Do you really want to delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        return this.lists.splice(index,1),
        this.$q.notify('ToDo Deleted')
      })
  },
  addTask() {
    this.lists.push({
      title: this.newTask,
      completed: false
    })
    this.newTask = ''
  }
}
})
</script>
<style lang="scss">
.done{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks{
  opacity: .5;
}
</style>