<template>
    <div class="container">
        <div class="row justify-content-center mt-5">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header bg-warning">Todo SPA (Laravel 8 Vue Js 2)</div>

                    <div class="card-body">
                        <div class="input-group">
                            <input type="text" placeholder="New Todo..." class="form-control" v-model="new_todo">
                            <div class="input-group-append">
                            <button v-if="!edit_todo_id" type="button" class="btn btn-success" @click="saveTodo()">Add</button>
                            <button v-else type="button" class="btn btn-success" @click="updateTodo()">Update</button>
                            <button type="button" class="btn btn-warning" @click="resetInput()">Reset</button>
                        </div>
                        </div>
                        
                        <table class="table mt-2 text-center">
                            <thead>
                                <tr>
                                    <th>S.N.</th>
                                    <th>Title</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(todo,index) in todos" :key="index">
                                    <td>{{++index}}</td>
                                    <td>{{todo.title}}</td>
                                    <td>
                                        <button type="button" class="btn btn-danger" @click="deleteTodo(--index)">Delete</button>
                                        <button type="button" class="btn btn-success" @click="editTodo(--index)">Edit</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                       
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
            todos:[],
            api:'http://127.0.0.1:8000/api/todos',
            new_todo:'',
            edit_todo_id:'',
            edit_index:'',
            }
        },
        mounted(){
            //get api data
            
            this.axios.get(this.api).then(res=>{
                this.todos=res.data;
            })
        },
        methods:{
            saveTodo(){
                if(this.new_todo.length > 0){
                    let d = {'title':this.new_todo};
                    this.axios.post(this.api,d).then(res=>{
                        this.todos.push(res.data);
                        this.new_todo='';
                    })
                }else{
                    alert('Input Something!');
                }
            },
            deleteTodo(index){
                if(this.todos[index].id){
                    this.axios.delete(this.api+'/'+this.todos[index].id).then(res=>{
                           this.todos.splice(index,1);
                    })
                }
            },
            editTodo(index){
                if(this.todos[index].id){

                    this.new_todo = this.todos[index].title;
                    this.edit_todo_id = this.todos[index].id;
                    this.edit_index = index;
                  
                }
            },
            resetInput(){
                this.new_todo = '';
                this.edit_todo_id = '';
                this.edit_index = '';
            },
            updateTodo(){
                if(this.new_todo.length > 0){
                    let d = {'title':this.new_todo};
                    this.axios.put(this.api+'/'+this.todos[this.edit_index].id,d).then(res=>{
            
                    this.todos[this.edit_index].title = res.data.title;
                        this.new_todo='';
                        this.edit_todo_id = '';
                    })
                }else{
                    alert('Input Something!');
                }
            }

        }
       
    }
</script>
