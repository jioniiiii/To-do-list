<template>
    <div class="container">
        <h2 class="text-center mt-5">My Vue Todo App</h2>

        <div class="d-flex">
            <input v-model="task" type="text" placeholder="Enter task" class="form-control">
            <button @click="submitTask()" class="btn btn-warning rounded-0">SUBMIT</button>
        </div>

        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                    <th scope="col" style="width: 635px;">Task</th>
                    <th scope="col" style="width: 125px;">Status</th>
                    <th scope="col" class="text-center" style="width: 185px;">#</th>
                    <th scope="col" class="text-center" style="width: 185px;">#</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task, index) in tasks" :key="index">
                    <td><span :class="{'finished': task.status === 'Finished'}">{{task.name}}</span></td>
                    <td>
                        <span class="pointer" @click="changeStatus(index)"
                            :class="{'text-danger': task.status === 'To-do',
                                    'text-warning': task.status === 'In-progress',
                                    'text-success': task.status === 'Finished'}"
                        >
                            {{task.status}}
                        </span>
                    </td>
                    <td>
                        <div class="text-center" @click="editTask(index)">
                            <span class="fa fa-pen"></span>
                        </div>
                    </td>
                    <td>
                        <div class="text-center" @click="deleteTask(index)">
                            <span class="fa fa-trash"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },

        data(){
            return{
                task: '',
                editedTask: null,
                availableStatus: ['To-do', 'In-progress', 'Finished'],

                tasks: [
                    {
                        name: 'fix motorbike',
                        status: 'To-do'
                    },

                    {
                        name: 'fix car',
                        status: 'In-progress'
                    }
                ]
            }
        },
        methods: {

           submitTask(){
                if(this.task.length === 0) return;

                if(this.editedTask === null){
                    this.tasks.push({
                    name: this.task,
                    status: 'To-do'
                })}
                else{
                    this.tasks[this.editedTask].name = this.task;
                    this.editedTask = null;
                }

                

                this.task = '';
           },

           deleteTask(index){
                this.tasks.splice(index, 1);
           }, 

           editTask(index){
            this.task = this.tasks[index].name;
            this.editedTask = index;
           },

           changeStatus(index){
            let newIndex = this.availableStatus.indexOf(this.tasks[index].status);
            if(++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.availableStatus[newIndex];
           }
        }
    }
</script>

<style scoped>
.pointer {
    cursor: pointer;
}

.finished {
    text-decoration: line-through;
}
</style>