<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodoItem="addOneItem"></TodoInput>
    <TodoList :propsdata="todoItems" @removeItem="removeOneItem" @toggleItem="toggleOneItem"></TodoList>
    <TodoFooter></TodoFooter>
    
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';
import TodoFooter from './components/TodoFooter.vue';

export default {
  components: {
    // 컴포넌트 태그명 : 컴포넌트 내용
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter,
  },
  data(){
    return {
      todoItems: []
    }
  },
  methods: {
    addOneItem(todoItem){
      const obj = {completed: false, item: todoItem};
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
            
    },
    removeOneItem(todoItem, index){
      console.log(todoItem,index);
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    toggleOneItem(todoItem, index){
      // todoItem.completed = !todoItem.completed;
      this.todoItems[index].completed = !this.todoItems[index].completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem))
    }
  },
  created(){
    if(localStorage.length == 0){
      return;
    }
    for(let i=0; i<localStorage.length; i++){
      if(localStorage.key(i) == 'loglevel:webpack-dev-server'){
        continue;
      }
      //this.todoItems.push(localStorage.key(i));
      console.log(JSON.parse(localStorage.getItem(localStorage.key(i))));
      this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))))

    }
  }
}
</script>

<style>
body {
  text-align : center;
  background-color : #f6f6f6;
}
input {
  border-style : groove;
  width : 200px;
}
button {
  border-style : groove;
}
.shadow {
  box-shadow : 5px 10px 10px rgba(0,0,0,0.03);

}
</style>
