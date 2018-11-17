<template>
    <div class="container">
        <h1>Lista de tareas</h1>
        <div class="task-in">
            <input type="text" placeholder="Nueva tarea" v-model="newTask" @keyup.enter="writeTask()"> 
            <button class="button blue" @click="writeTask()">
                 Añadir    
                 <font-awesome-icon class="icon" icon="plus" />
            </button>  
        </div>
        <div class="buttons">
            <button class="button green">
                Borrar completados
                <font-awesome-icon class="icon" icon="check" />
            </button>
            <button class="button red">
                Borrar todos
                 <font-awesome-icon class="icon" icon="trash" />
            </button>
        </div> 
        <tasks ref="tasks"></tasks>
    </div>
</template>

<script>
import axios from 'axios';
import tasks from './task.vue';

export default {
        components: {
            tasks
        },
        data () {
            return {
                newTask: ''
            }
        },
        methods: {
            writeTask () {
                const today = new Date();
                const task = {
                    creationDate: today,
                    task: this.newTask
                }
                
                axios.post('https://todo-list-memo.firebaseio.com/tasks.json', task)
                    .then(response => {
                        this.$refs.tasks.updateTasks();
                    })
                    .catch(error => console.log(error));
                
                this.newTask = '';
                
            }
        }
}
</script>

<style scoped>

.container {
    height: 30em;
    width: 40em;
    margin-top: 6em;
    background-color: #ffffff;
    border-radius: 1%;
    box-shadow: -5px 6px 30px rgba(78, 78, 78, 0.534);
}

.task-in {
    width: 100%;
    height: 4em;
    display: flex;
    align-items: center;

}

.container .task-in input[type=text] {
    width: 100%;
    height: 3em;
    margin: 10px;
    border-radius: 1%;
    border: 1px solid #b3b3b3;
    padding-left: 20px;

}

.button {
    width: 20%;
    height: 3em;
    border: none;
    color: white;
    margin: 10px; 
    font-size: 14px;       
}

.buttons button {
    width: 30%;
}

.buttons {
    text-align: right;
}

.icon {
    margin-left: 8px;
}


</style>
