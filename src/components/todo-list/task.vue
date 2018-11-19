<template>
    <div>
        <ul>
            <li v-for="(task, index) in tasks" class="task">
                <button 
                    class="task-item" 
                    @click="updateCompleted(index)">
                        <font-awesome-icon 
                            v-if="task.completed" 
                            class="icon" 
                            icon="check"/>
                        <span
                            class="task-text">
                            {{ task.task }}
                        </span>
                 </button>
                <button class="delete red" @click="deleteTask(task.id)">
                    <font-awesome-icon class="icon" icon="times" />
                </button>
            </li>
        </ul>
        {{ tasks }} 

    </div>
</template>


<script>
import axios from 'axios';
import swal from 'sweetalert2';

export default {
    data () {
        return {
            tasks: []
        }
    },
    methods: {
        getTasks() {
             axios.get('https://todo-list-memo.firebaseio.com/tasks.json')
                .then(response => {
                    const db_tasks = response.data;
                    const ids = Object.keys(db_tasks); 
                    let i = 0;

                    this.tasks = [];

                    for (const key in db_tasks) {
                        let temp = {
                            id: ids[i],
                            task: db_tasks[key].task,
                            completed: db_tasks[key].completed
                        };
                        this.tasks.push(temp);
                        i++;
                    }  
                })
                .catch(error => console.log(error));
        },
        updateTasks() {
           
            this.getTasks();                
        },
        updateCompleted(index) {
            this.tasks[index].completed = !this.tasks[index].completed;
        },
        deleteTask(id) {
             swal({
                        title: 'Borrar registro?',
                        confirmButtonText: "Borrar",
                        confirmButtonColor: "#cc0000",
                        showCancelButton: true
                    }).then((result) => {
                        if (result.value) {
                        
                        axios.delete('https://todo-list-memo.firebaseio.com/tasks/' + id + '.json')
                            .then(response => {
                                 swal({
                                    type: 'success',
                                    title: 'Tarea borrada'
                                });
                                this.updateTasks();
                            })
                            .catch(error => console.log(error));

                       
                        }
                        
                    });
        }
    },
    created () {
        this.getTasks();
    }
}
</script>

<style scoped>

ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
}

button {
    height: 45px;
    border: none;
    color: white;
    margin: 10px; 
    font-size: 16px;  
}

.task {
    display: flex;
}

.task-item {
     width: 100%; 
     margin-right: 0; 
     padding-left: 10px;
     text-align: left;
     background-color: #e4e5e7;
     color: #727781;
     display: grid;
     grid-template-columns: 30px 1fr;
}

.task-item:hover {
    background-color: #dadbde;
}

.delete {
    width: 50px;
    margin-left: 0;
    padding: 0;
}

.icon {
    grid-column: 0 / 1;
}

.task-text {
    grid-column: 1 / 2;
}

</style>
