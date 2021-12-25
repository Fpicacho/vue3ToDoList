<template>
  <!--待办输入框-->
  <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo"/>
  <!--代办列表-->
  <ul>
    <li v-for="item in ToDoList" :key="item.id">
      {{item.content}} <button @click="deleteToDo(item.id)">X</button></li>
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
            id:state.ToDoList.length,
            content:state.newToDo,
            isDone:false
          }
      )
      state.newToDo = ''
    }
    function deleteToDo(index){
      state.ToDoList.splice(index,1)
    }
    return {
      ...toRefs(state),
      addToDo,
      deleteToDo
    }
  }
}
</script>

<style scoped>

</style>