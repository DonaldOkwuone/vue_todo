<template>
  <div id="app">  
    <AddTodo v-on:add-todo="addTodo" />
    <Todo v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todo from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components: {  
    Todo,
    AddTodo
  },
  data(){
    return{
      todos:[
       
      ]
    }
  },
  methods:{
    deleteTodo(id){
        console.log(id);// eslint-disable-line no-console
        axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => this.todos = this.todos.filter(todo => todo.id !== id) )// eslint-disable-line no-unused-vars
        .catch(err => console.log(err));// eslint-disable-line no-console
        //axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`).then( this.todos = this.todos.filter(todo => todo.id !== id) ).catch();
        //this.todos = this.todos.filter(todo => todo.id !== id);
    },
    addTodo(newTodo){
      const{title, completed} = newTodo;
      //use spread operator
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title, 
        completed
        } )// eslint-disable-line no-console
      .then(res => this.todos = [...this.todos, res.data] ).then(res => console.log(res))// eslint-disable-line no-console
      .catch( err => console.log(err));// eslint-disable-line no-console
      //console.log(this.todos);// eslint-disable-line no-console
      //this.todos = [ ...this.todos, newTodo]
    }   
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5').then(res => this.todos = res.data).catch();
  }
}
</script>

<style>
  * {
      box-sizing: border-box;
      margin:0;
      padding:0;
  }
  body {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      line-height: 1.4;
  }
  .btn {
      display: inline-block;
      border:none;
      background: #555;
      color:#fff;
      padding:7px 20px;
      cursor:pointer;
  }
  .btn:hover{
    background: #666;
  }
</style>
