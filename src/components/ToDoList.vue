<template>
  <!--待办输入框-->
  <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo"/>
  <!--代办列表-->
  <ul>
    <li v-for="item in ToDoList" :key="item.id" @dblclick="changeToDo(item.id)">
      {{ item.content }}
      <button @click.stop="deleteToDo(item)">X</button>
      <input type="text" v-model="oldToDo"
             @keyup.esc.stop="serveChangeToDo(item.id)"
             @keyup.enter="serveChangeToDo(item.id)"
             @blur="serveChangeToDo(item.id)">
    </li>
  </ul>
</template>

<script>
import {reactive, toRefs} from 'vue'

export default {
  name: "ToDoList",
  setup() {
    const state = reactive({
      ToDoList: [],
      newToDo: "",
      oldToDo: "",
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
    function deleteToDo(item) {
      state.ToDoList.splice(state.ToDoList.indexOf(item),1)
    }

    // 选择修改todo
    function changeToDo(index) {
      state.oldToDo = state.ToDoList[index].content
    }

    // 修改todo
    function serveChangeToDo(index) {
      state.ToDoList[index].content = state.oldToDo
      state.oldToDo = ""
    }

    return {
      ...toRefs(state),
      addToDo,
      deleteToDo,
      changeToDo,
      serveChangeToDo
    }
  }
}
</script>

<style scoped>

</style>