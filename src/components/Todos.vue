<template>
    <div class="container">
    <h1>list of todos</h1>

        <div class="form-group">
            <input type="text"  class=" form-control"  placeholder="Add Todo ..." v-model="title">
        </div>
    <button  v-if="myTodo" @click="updateTodo" class="btn btn-warning   btn-block  mb-4">
        Update Todo
    </button>
        <button v-else @click="addTodo" class="btn btn-success btn-block  mb-4">
        Add Todo
    </button>

    <ul class="list-group">
        <li class="list-group-item" v-for=" todo in todos" v-bind:key="todo.id" > 
           <Todo  :todo="todo" 
           v-on:deteleTodo="deleteTodo(todo.id)"
           @updateTodo="editTodo"
           /> 

        </li>

    </ul>

    </div>
</template>

<script>
import axios from 'axios';
import Todo from './Todo'; 
export default {
    name: "Todos",
    components:{
        Todo
    },
    data(){
        return{
            title: '',
            completed: false,
            myTodo:'',
            todos:[]
        }
    },
    methods:{
        getTodos(){
            
            axios.get('http://localhost:3004/todos')
                    .then(res => this.todos = res.data)
                    .catch(err => console.log(err))
        },
        deleteTodo(id)
        { 
            axios.delete(`http://localhost:3004/todos/${id}`)
                .then(() => {
                     this.todos = this.todos.filter(todo => todo.id !== id )   
                })
        },
        addTodo()
        {
           // e.preventDefault();
            if(this.title != '')
            {
                let newTodo = {
                title: this.title,
                completed: false
            }
            axios.post('http://localhost:3004/todos',newTodo)
            .then(res => {
                   this.todos =  [res.data, ...this.todos];
                   this.title = '';
            })
            }
        },
        editTodo(todo)
        {
            this.myTodo = todo;
            this.title = todo.title; 
        },
        updateTodo()
        {   
            let todo =
            {
                ...this.myTodo,
                title:this.title    
            }
            axios.put('http://localhost:3004/todos/'+todo.id,todo)
                    .then(res => {
                        this.todos =  this.todos.map(t =>{
                            if(res.data.id === t.id)
                            {
                                return res.data
                            }
                            return t;
                          
                        })
                          this.title= '';
                            this.myTodo = null;
                    })
        }

    },
    created()
    {
        this.getTodos();
    }
}
</script>

<style>

</style>
