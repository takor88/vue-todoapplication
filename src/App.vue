<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @addTodoItem="addOneItem"></TodoInput>
    <TodoList :propsdata="todoItems" @removeTodoItem="removeOneItem" @toggleTodoItem="toggleOneItem"></TodoList>
    <TodoFooter @clearAll="removeAllItems"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';
import TodoFooter from './components/TodoFooter.vue';

export default {
  data(){
    return{
      todoItems:[],
    }
  },
  created(){
    if(localStorage.length > 0){
      for(let i = 0; i< localStorage.length; i++){
        if(localStorage.key(i)!='loglevel:webpack-dev-server'){
          var obj = JSON.parse(localStorage.getItem(localStorage.key(i)));
          this.todoItems.push(obj);
        }
      }
    }
  },
  methods:{
    addOneItem: function(todoItem){
      var obj = {completed: false, item: todoItem}
      localStorage.setItem(todoItem, JSON.stringify(obj));
      this.todoItems.push(obj);
    },
    removeOneItem: function(todoItem, index){
      localStorage.removeItem(todoItem.item);
      this.todoItems.splice(index, 1);
    },
    removeAllItems: function(){
      localStorage.clear();
      this.todoItems = [];
    },
    toggleOneItem: function(todoItem,index){
      this.todoItems[index].completed = !this.todoItems[index].completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
  },
  components:{
    'TodoHeader': TodoHeader,
    'TodoInput':TodoInput,
    'TodoList':TodoList,
    'TodoFooter':TodoFooter
  }
}
</script>
<style>
body {
  text-align: center;
  background-color: #F6F6F6;
}
input{
  border-style: groove;
  width: 200px;
}
button{
  border-style: groove;
}
.shadow{
  box-shadow: 5px 10px 10px rgba(0,0,0,0.05);
}
</style>