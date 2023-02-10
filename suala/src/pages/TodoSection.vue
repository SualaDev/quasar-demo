<template>
  <q-list>
      <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->

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
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data(){
  return {
    lists: [
      {
        title: "Clean the Car",
        completed: false
      },
      {
        title: "Code",
        completed: false
      },
      {
        title: "Gym",
        completed: false
      }
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
</style>