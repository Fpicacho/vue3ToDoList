<template>
  <div class="todoListPage">
    <h1>ToDoList</h1>
    <!--待办输入框-->
    <div class="todoInputBox">
      <span>+</span>
      <input type="text" placeholder="请输入代办事项" v-model="newToDo" @keyup.enter="addToDo" class="todoInput"/>
    </div>
    <!--代办列表-->
    <ul>
      <li v-for="item in filterTodoList" :key="item.id" @dblclick.stop="changeToDo(item)"
          :class="{isEdit:item === modifiedItem}">
        <div class="todoInput">
          <button @click.stop="deleteToDo(item)">X</button>
          <input type="checkbox" v-model="item.isDone">
          <span :class="{doneTure:item.isDone}">{{ item.content }}</span>
        </div>
        <input
            class="reviseInput"
            type="text"
            v-model="oldToDo"
            v-todo-focus="item === modifiedItem"
            @keyup.esc="serveChangeToDo(item)"
            @blur="serveChangeToDo(item)"
            @keyup.enter.stop="serveChangeToDo(item)">
      </li>
    </ul>
    <div>
      <button @click="filterTodo('all')">全部</button>
      <button @click="filterTodo('DoneTrue')">已完成</button>
      <button @click="filterTodo('DoneFalse')">未完成</button>
    </div>
  </div>
</template>

<script>
import {reactive, toRefs, watchEffect,onMounted} from 'vue'

const todoStorage = {
  fetch() {
    let todos = JSON.parse(localStorage.getItem('vue3-todos') || "[]")
    todos.forEach((todo, index) => {
      todo.id = index + 1
    })
    return todos
  },
  save(todos) {
    localStorage.setItem('vue3-todos',JSON.stringify(todos))
  }
}

export default {
  name: "ToDoList",
  setup() {
    const state = reactive({
      filterTodoList: [],
      ToDoList: todoStorage.fetch(),
      newToDo: "",
      oldToDo: "",
      modifiedItem: ""
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
    function changeToDo(item) {
      // state.ToDoList[index].isEdit = !state.ToDoList[index].isEdit
      // state.oldToDo = state.ToDoList[index].content
      state.modifiedItem = item
      state.oldToDo = item.content
    }

    // 修改todo
    function serveChangeToDo(item) {
      item.content = state.oldToDo
      state.modifiedItem = ""
    }

    // 筛选
    function filterTodo(type) {
      if (type === 'all') {
        state.filterTodoList = state.ToDoList.filter((item) => {
          return item
        })
      } else if (type === 'DoneTrue') {
        state.filterTodoList = state.ToDoList.filter((item) => {
          return item.isDone === true
        })
      } else {
        state.filterTodoList = state.ToDoList.filter((item) => {
          return item.isDone === false
        })
      }
    }
    onMounted(()=>{
      state.filterTodoList = state.ToDoList
    })
    watchEffect(()=>{
      todoStorage.save(state.ToDoList)
    })
    return {
      ...toRefs(state),
      addToDo,
      deleteToDo,
      changeToDo,
      serveChangeToDo,
      filterTodo,
    }
  },
  directives: {
    "todo-focus": (el, {value}) => {
      if (value) {
        el.focus()
      }
    }
  }
}
</script>

<style scoped lang="scss">
.doneTure {
  text-decoration: line-through;
}

.reviseInput {
  display: none;
}

.isEdit .reviseInput {
  display: block;
}

.isEdit .todoInput {
  display: none;
}

.todoListPage{
  padding: 10vh 20vw;
  h1{
    color: #fff;
    padding: 2vh 0;
  }
  input{
    background: #181820;
    border: 2px solid rgba(46, 46, 63, 0.92);
    border-radius: 10px;
    width: 100%;
    //padding: 2vh 4vh;
    height: 5vh;
    text-indent: 2.5vw;
  }
  .todoInputBox{
    position: relative;
    span{
      position: absolute;
      display: block;
      background: #fc76a1;
      height: 3vh;
      width: 3vh;
      border-radius: 15%;
      font-size: 3vh;
      text-align: center;
      line-height: 3vh;
      top:50%;
      transform: translateY(-50%);
      left: 1.5vh;
    }
    input{
      //padding:2vh 3vw;
      font-size: 1.5vh;
      color: white;
    }
  }
  ul{
    li{
      margin-top: 1vh;
      background: #21212b;
    }
  }
}

</style>