<template>
  <div class="wrapper">
    <h1>Task Tool</h1>
    <p class="sub-header">Adding whatever you want!</p>
    <div class="input-search">
      <input type="text" v-model="inputTitle" v-on:keyup.enter="addList()" />
      <button type="button" v-on:click="addList()">
        <svg width="36" height="36" version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48">
          <path d="M33,19 L33,27 L15,27" stroke="#AAAAAA" fill="transparent" stroke-width="2"></path>
          <polygon points="15 23, 15 31, 9 27" stroke="#AAAAAA" fill="#AAAAAA" stroke-width="2"/>
        </svg>
      </button>
    </div>
    <ul class="list-content">
      <ol v-for="(item, idx) in getList()" :key="idx" :class="[ 'list-item', item.checked ? 'checked' : '' ]">
        
        <label class="item-label" style="width: 100%;">
          <input type="checkbox" style="width:0;height:0;" @change="checkItem(idx)"/>
          <span class="checkmark">
            <svg width="72" height="72" version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path d="M2,12 L6,18" stroke="#fff" fill="transparent" stroke-width="3"></path>
              <path d="M4.5,18 L15,5" stroke="#fff" fill="transparent" stroke-width="3"></path>
            </svg>
          </span>
          <span>{{ item.title }}</span>
        </label>
        <span @click="removeItem(idx)">
          <svg width="36" height="36" version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <path d="M6,6 L18,18" stroke="#AAAAAA" fill="transparent" stroke-width="2"></path>
            <path d="M6,18 L18,6" stroke="#AAAAAA" fill="transparent" stroke-width="2"></path>
          </svg>
        </span>

      </ol>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, nextTick } from 'vue'

  interface Model{
    todoList: CheckItem[]
  }

  interface CheckItem{
    title: string
    checked: boolean
  }
  export default defineComponent({
    data() {
      return {
        itemList: <CheckItem[]>[],
        inputTitle: <string>''
      }
    },
    methods:{
      getList() {
        return this.itemList;
      },
      addList(): void {
        if (this.inputTitle != '') {
          this.itemList.push({title: this.inputTitle, checked: false});
          this.inputTitle = '';
        }

        // calc the items x list count to get height for scroll element
        let itemHeight = document.querySelector(".list-item")?.clientHeight ?? 0;
        let contentHeight = this.itemList.length * itemHeight;
        nextTick(() => {
          // scroll to bottom of the list
          document.querySelector(".list-content")?.scroll({ top: contentHeight, behavior: 'smooth' });
        });
      },
      checkItem(idx: number) {
        // reverse flags
        this.itemList[idx].checked = !this.itemList[idx].checked;
      },
      removeItem(idx: number) {
        // remove specific item
        this.itemList.splice(idx, 1);
      }
    },
    mounted() {

    }
  });
</script>
