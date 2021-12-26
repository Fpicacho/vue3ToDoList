<template>
  <!--待办输入框-->
  <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo"/>
  <!--代办列表-->
  <ul>
    <li v-for="item in filterTodoList" :key="item.id" @dblclick.stop="changeToDo(item.id)">
      <div v-if="!item.isEdit">
        <input type="checkbox" v-model="item.isDone">
        <span :class="{doneTure:item.isDone}">{{ item.content }}</span>
        <button @click.stop="deleteToDo(item)">X</button>
      </div>
      <input
          v-if="item.isEdit"
          type="text"
          v-model="oldToDo"
          @keyup.esc="serveChangeToDo(item.id)"
          @keyup.enter="serveChangeToDo(item.id)"
          @blur="serveChangeToDo(item.id)">
    </li>
  </ul>
  <div>
    <button @click="filterTodo('all')">全部</button>
    <button @click="filterTodo('DoneTrue')">已完成</button>
    <button @click="filterTodo('DoneFalse')">未完成</button>
  </div>
</template>

<script>
import {reactive, toRefs} from 'vue'

export default {
  name: "ToDoList",
  setup() {
    const state = reactive({
      filterTodoList:[],
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
            isDone: false,
            isEdit: false
          }
      )
      state.newToDo = ''
      state.filterTodoList = state.ToDoList
    }

    // 删除todo
    function deleteToDo(item) {
      state.ToDoList.splice(state.ToDoList.indexOf(item), 1)
    }

    // 选择修改todo
    function changeToDo(index) {
      state.ToDoList[index].isEdit = !state.ToDoList[index].isEdit
      state.oldToDo = state.ToDoList[index].content
    }

    // 修改todo
    function serveChangeToDo(index) {
      state.ToDoList[index].isEdit = false
      state.ToDoList[index].content = state.oldToDo
    }

    // 筛选
    function filterTodo(type){
      if (type === 'all'){
         state.filterTodoList = state.ToDoList.filter((item)=>{
           return item
        })
      }else if(type === 'DoneTrue'){
        state.filterTodoList = state.ToDoList.filter((item)=>{
          return item.isDone === true
        })
      }else{
        state.filterTodoList = state.ToDoList.filter((item)=>{
          return item.isDone === false
        })
      }
    }

    return {
      ...toRefs(state),
      addToDo,
      deleteToDo,
      changeToDo,
      serveChangeToDo,
      filterTodo
    }
  }
}
</script>

<style scoped >
.doneTure{
  text-decoration: line-through;
}
</style>