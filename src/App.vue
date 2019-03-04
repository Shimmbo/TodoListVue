<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-6">
        <div class="todolist not-done">
          <h1>Todos</h1>
          <div class="input-group">
            <input v-model="newTask" type="text" class="form-control add-todo" placeholder="Add todo">
            <span class="input-group-btn">
              <button @click="addToList()" class="btn btn-default">Add</button>
            </span>
          </div>
          <hr>
          <ul class="list-unstyled">
            <TodoListItem
              v-for="todo in allTodoList"
              :key="todo.Id"
              :todoItem="todo"
              @remove='removeTodo'
            />
          </ul>
          <div class="todo-footer">
              <strong><span class="count-todos"></span></strong> Items Left: {{allTodoList.length}}
          </div>
        </div>
      </div>
      <div class="col-md-6">
          <div class="todolist">
            <h1>Already Done</h1>
            <ul id="done-items"  class="list-unstyled"> 
              <TodoListItem
                v-for="todo in completedList"
                :key="todo.Id"
                :todoItem="todo"
                @remove='removeTodo'
              />
            </ul>
          </div>
      </div>
    </div>
  </div>
</template>

<script>
import TodoListItem from './components/TodoListItem.vue'

export default {
  mounted(){
    if (localStorage.getItem('TODOS-LIST')) 
      this.todoList = JSON.parse(localStorage.getItem('TODOS-LIST'));
  },
  data(){
    return {
      newTask: '',
      todoList: [],
    }
  },
  methods:{
    addToList(){
      if(this.newTask == '') return;

      var nextId = this.getNextId();
      nextId++;
      this.todoList.push({
        Id: nextId,
        Description: this.newTask,
        Completed: false
      });
      this.newTask = '';
    },
    getNextId(){
      var max = 0;
      for(var i = 0; i < this.todoList.length; i++){
        max = Math.max(this.todoList[i].Id, max);
      }
      return max;
    },
    removeTodo(todoItem){
      this.todoList = this.todoList.filter(function(x) {
        return x.Id != todoItem.Id;
      })
    }
  },
  computed: {
    allTodoList(){
      return this.todoList.filter(function(x) {return !x.Completed})
    },
    completedList(){
       return this.todoList.filter(function(x) {return x.Completed})
    }
  },
  watch: {
    todoList: {
      handler() {
        localStorage.setItem('TODOS-LIST', JSON.stringify(this.todoList));
      },
      deep: true,
    },
  },
  components: {
    TodoListItem
  }
}
</script>
<style scoped>
body{
    background-color:#EEEEEE;
}
.todolist{
    background-color:#FFF;
    padding:20px 20px 10px 20px;
    margin-top:30px;
}
.todolist h1{
    margin:0;
    padding-bottom:20px;
    text-align:center;
}
.form-control{
    border-radius:0;
}
li.ui-state-default{
    background:#fff;
    border:none;
    border-bottom:1px solid #ddd;
}

li.ui-state-default:last-child{
    border-bottom:none;
}

.todo-footer{
    background-color:#F4FCE8;
    margin:0 -20px -10px -20px;
    padding: 10px 20px;
}
#done-items li{
    padding:10px 0;
    border-bottom:1px solid #ddd;
    text-decoration:line-through;
}
#done-items li:last-child{
    border-bottom:none;
}
#checkAll{
    margin-top:10px;
}
</style>

