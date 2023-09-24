<template>
  <div class="wrapper">
    <h1 class="green">Task Tool</h1>
    <h3>
      Adding whatever you want! 
    </h3>
    <input type="text" v-model="taskTitle" v-on:keyup.enter="addList()" />
    <ul>
      <ol v-for="item in getList()">
        {{ item }}
      </ol>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

  interface Model{
    todoList: string[]
  }
  export default defineComponent({
    data() {
      return {
        todoList: <string[]>[],
        taskTitle: <string>''
      }
    },
    methods:{
      getList() {
        return this.todoList;
      },
      addList(): void {
        console.log('123');
        if (this.taskTitle != '') {
          this.todoList.push(this.taskTitle);
          this.taskTitle = '';
        }
      }
    },
    mounted() {

    }
  });
</script>
