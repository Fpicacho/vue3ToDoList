<template>
  <!--待办输入框-->
  <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo"/>
  <!--代办列表-->
  <ul>
    <li v-for="item in ToDoList" :key="item.id" @click="changeToDo(item.id)">
      {{ item.content }}
      <button @click.stop="deleteToDo(item.id)">X</button>
      <input type="text" @keyup.esc.stop="changeToDo(item.id)" @blur="changeToDo(item.id)" v-model="newToDo">
    </li>
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

    // 增加todo
    function addToDo() {
      state.ToDoList.push(
          {
            id: state.ToDoList.length,
            content: state.newToDo,
            isDone: false
          }
      )
      state.newToDo = ''
    }

    // 删除todo
    function deleteToDo(index) {
      state.ToDoList.splice(index, 1)
    }

    // 修改todo
    function changeToDo(index) {
      state.ToDoList[index].content = state.newToDo
      state.newToDo = ''
    }

    return {
      ...toRefs(state),
      addToDo,
      deleteToDo,
      changeToDo
    }
  }
}
</script>

<style scoped>

</style>