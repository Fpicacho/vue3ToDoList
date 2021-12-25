<template>
  <!--待办输入框-->
  <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo"/>
  <!--代办列表-->
  <ul>
    <li v-for="item in ToDoList" :key="item.id">{{item.content}}</li>
  </ul>
</template>

<script>
import {reactive, toRefs} from 'vue'

export default {
  name: "ToDoList",
  setup() {
    const state = reactive({
      newToDo: "",
      ToDoList: []
    })
    function addToDo(){
      state.ToDoList.push(
          {
            id:state.ToDoList.length + 1,
            content:state.newToDo,
            isDone:false
          }
      )
      state.newToDo = ''
    }
    return {
      ...toRefs(state),
      addToDo
    }
  }
}
</script>

<style scoped>

</style>