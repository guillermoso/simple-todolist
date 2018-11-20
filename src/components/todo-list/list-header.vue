<template>
    <div class="container" id="container">
        <h1>Lista de tareas</h1>
        <div class="task-in">
            <input type="text" placeholder="Nueva tarea" v-model="newTask" @keyup.enter="writeTask()"> 
            <button class="button blue" @click="writeTask()">
                 Añadir    
                 <font-awesome-icon class="icon" icon="plus" />
            </button>  
        </div>
        <div class="buttons">
            <button class="button green" @click="deleteCompleted()">
                Borrar completados
                <font-awesome-icon class="icon" icon="check" />
            </button>
            <button class="button red" @click="deleteAll()">
                Borrar todos
                 <font-awesome-icon class="icon" icon="trash" />
            </button>
        </div> 
        <tasks 
            ref="tasks"
            ></tasks>
    </div>
</template>

<script>
import axios from 'axios';
import tasks from './task.vue';
import swal from 'sweetalert2';

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
                    task: this.newTask,
                    completed: false
                }
                
                axios.post('https://todo-list-memo.firebaseio.com/tasks.json', task)
                    .then(response => {
                        this.$refs.tasks.updateTasks();
                    })
                    .catch(error => console.log(error));
                
                this.newTask = '';                
            },
            deleteAll() {
                 swal({
                        title: 'Borrar todos?',
                        confirmButtonText: "Borrar todos",
                        confirmButtonColor: "#cc0000",
                        showCancelButton: true
                    }).then((result) => {
                        
                        if (result.value) {
                        
                        axios.delete('https://todo-list-memo.firebaseio.com/tasks.json/')
                            .catch(error => console.log(error));

                        swal({
                            type: 'success',
                            title: 'Se borraron todas las tareas'
                        });
                        this.$refs.tasks.tasks = [];
                        }
                        
                    });
                   
            },
            deleteCompleted() {
                this.$refs.tasks.deleteCompleted();
            }
        }
}
</script>

<style scoped>

.container {
    height: 495px;
    width: 640px;
    margin-top: 90px;
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
