<template>
    <div>
        <ul>
            <li v-for="(task, index) in tasks" class="task">
                <button 
                    class="task-item" 
                    @click="markCompleted(index, task.id)">
                        <span
                            class="task_text">
                            {{ task.task }}
                        </span>
                        <font-awesome-icon 
                            v-if="task.completed" 
                            class="icon" 
                            icon="check"/>
                 </button>
                <button class="delete red" @click="deleteTask(task.id)">
                    <font-awesome-icon class="icon" icon="times" />
                </button>
            </li>
        </ul>
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

                    if (response.data == null) {
                           this.tasks = [];
                    } else {
                        const db_tasks = response.data;
                        const ids = Object.keys(db_tasks); 
                        let i = 0;

                        this.tasks = [];

                         for (const key in db_tasks) {
                            let temp = {
                                id: ids[i],
                                creationDate: db_tasks[key].creationDate,
                                task: db_tasks[key].task,
                                completed: db_tasks[key].completed
                            };
                            this.tasks.push(temp);
                            i++;
                         }
                    }

                    this.adjustHeight();
                    
                })
                .catch(error => console.log(error));
        },
        updateTasks() {
            this.getTasks();                
        },
        markCompleted(index, id) {
            this.tasks[index].completed = !this.tasks[index].completed;

            axios.put('https://todo-list-memo.firebaseio.com/tasks/' + id +'.json', 
                    {
                        creationDate: this.tasks[index].creationDate,
                        task: this.tasks[index].task,
                        completed: this.tasks[index].completed
                    })
                    .catch(error => console.logs(error));

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
        },
        deleteCompleted() {
            let indexCompleted = [];

            for (let i = 0; i < this.tasks.length; i++) {
                if (this.tasks[i].completed == true) {
                    indexCompleted.push(i);        
                }
            }

            if (indexCompleted.length != 0) {
                for (let i = 0; i < indexCompleted.length; i++) {

                    let id = this.tasks[indexCompleted[i]].id;

                    if (i == indexCompleted.length - 1) {
                        axios.delete('https://todo-list-memo.firebaseio.com/tasks/' + id + '.json')
                                .then(response => {
                                    this.getTasks();
                                })
                                .catch(error => console.log(error)); 
                    } else {
                        axios.delete('https://todo-list-memo.firebaseio.com/tasks/' + id + '.json')
                                .catch(error => console.log(error));  
                    }
                }

                swal({
                    type: 'success',
                    title: 'Tareas completadas borradas'
                });
            } else {
                swal({
                    type: 'warning',
                    title: 'No hay tareas completadas'
                });
            }
            
            
        },
        adjustHeight() {
             if (this.tasks.length > 5) {
                        console.log('entra');
                        const excedingTasks = this.tasks.length - 5;
                        const totalHeight = 495 + (excedingTasks * 54);

                        console.log(totalHeight);
                        document.getElementById("container").style.height = totalHeight + "px";
                    }
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
    margin: 10px 10px 0 10px; 
    font-size: 16px;  
}

.task {
    display: flex;
}

.task-item {
     width: 100%; 
     margin-right: 0; 
     padding-left: 20px;
     text-align: left;
     background-color: #e4e5e7;
     color: #727781;         
}

.task-item:hover {
    background-color: #dadbde;
}

.delete {
    width: 50px;
    margin-left: 0;
    padding: 0;
}

</style>
