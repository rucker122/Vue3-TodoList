<template>
  <div class="wrapper">
    <h1>Task Tool</h1>
    <p class="sub-header">Adding whatever you want!</p>
    <div class="input-search">
      <input type="text" v-model="taskTitle" v-on:keyup.enter="addList()" />
      <button type="button" v-on:click="addList()">
        <svg width="36" height="36" version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48">
          <path d="M33,19 L33,27 L15,27" stroke="#AAAAAA" fill="transparent" stroke-width="2"></path>
          <polygon points="15 23, 15 31, 9 27" stroke="#AAAAAA" fill="#AAAAAA" stroke-width="2"/>
        </svg>
      </button>
    </div>
    <ul class="list-content">
      <ol class="list-item" v-for="item in getList()" v-bind:key="item">
        {{ item }}
      </ol>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, nextTick } from 'vue'

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
        if (this.taskTitle != '') {
          this.todoList.push(this.taskTitle);
          this.taskTitle = '';
        }
        // calc the items x list count to get height for scroll element
        let itemHeight = document.querySelector(".list-item")?.clientHeight ?? 0;
        let contentHeight = this.todoList.length * itemHeight;
        nextTick(() => {
          // scroll to bottom of the list
          document.querySelector(".list-content")?.scroll({ top: contentHeight, behavior: 'smooth' });
        });
      }
    },
    mounted() {

    }
  });
</script>
