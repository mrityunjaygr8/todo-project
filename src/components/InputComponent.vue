<template>
    <div class="container">
        <div class="d-flex">
            <input v-model="task" type="text" placeholder="enter the task" class="form-control">
            <button @click="submitTask" class="btn btn-dark">Submit</button>
        </div>

        <table class="table table-hover table-bordered mt-5">
            <thead>
                <th>Task</th>
                <th>Status</th>
                <th>Update</th>
                <th>Delete</th>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td>{{task.name}}</td>
                    <td>{{task.status}}</td>
                    <td><div @click="editTask(index)" class="pointer"><span class="fas fa-pen-fancy"></span></div></td>
                    <td><div @click="deleteTask(index)" class="pointer"><span class="fas fa-trash-alt"></span></div></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
//import * as fb from '@/firebaseApi.js'
export default {
    name: 'InputComponent',
    data(){
        return{
            task:"",
            tasks:[
                {
                    name:"Do work of the day",
                    status:"to-do"
                },
                {
                    name:"Finish the task before 5",
                    status:"to-do"
                }
            ]
        }
    },
    methods:{
        submitTask(){
            if(this.task.length === 0){
                alert("please enter the task");
            }
            this.tasks.push({
                name: this.task,
                status: "to-do"
            })
            this.task= '';
           // console.warn(fb.db.tasksCollection)
        },
        editTask(index){
            this.task = this.tasks[index].name;
        },
        deleteTask(index){
            this.tasks.splice(index, 1)
        }
    }
}
</script>

<style>
.pointer{
    cursor: pointer;
}
</style>