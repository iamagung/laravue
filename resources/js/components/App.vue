<template>
    <div class="app-component">

        <table class="table">
            <thead>
                <tr>
                    <th> ID </th>
                    <th> Task </th>
                    <th> Priority </th>
                    <th> Action </th>
                </tr>
            </thead>
            <tbody>
                <task-component v-for="task in tasks" :key="task.id" :task="task" @delete="remove"></task-component>

                <tr>
                    <td><input v-model="task.title" type="text" id="task" class="form-controll"></td>
                    <td>
                        <select v-model='task.priority' id="select" class="form-control">
                            <option> Low </option>
                            <option> Medium </option>
                            <option> High </option>
                        </select>
                    </td>
                    <td><button @click="store" class="btn btn-primary"> ADD </button></td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>

    import TaskComponent from './Task';

    export default {
        
        data() {
            
            return {
                
                tasks: [],
                task: { title: '', priority: ''}
            }

        },

        methods: {

            getTasks(){

                window.axios.get('/api/tasks').then(({data}) =>{
                    data.forEach((task) => {
                        this.tasks.push(task)
                    });
                });
            },

            store(){
                if(this.checkInputs()) {
                    window.axios.post('/api/tasks', this.task).then(savedTask =>{
                        this.tasks.push(savedTask.data);
                        this.task.title = '';
                    });
                }
            },

            checkInputs(){
                if (this.task.title && this.task.priority) return true;
            },

            remove(id){
                window.axios.delete(`/api/tasks/${id}`).then(() => {
                    let index = this.tasks.findIndex(task => task.id === id)
                    this.tasks.splice(index, 1);
                });

                console.log(`I GOT THE ATA ${id}`);
            }
        },

        created() {
            this.getTasks();
        },

        components: {TaskComponent}
    }
</script>

<style>
    
</style>