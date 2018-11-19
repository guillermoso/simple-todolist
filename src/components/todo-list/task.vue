<template>
    <div>
        <ul>
            <li v-for="task in tasks" class="task">
                <button class="task-item" @click="updateCompleted()"> {{ task.task }} </button>
                <button class="delete red">
                    <font-awesome-icon class="icon" icon="times" />
                </button>
            </li>
        </ul>
        {{ tasks }} 

    </div>
</template>


<script>
import axios from 'axios';

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
            this.tasks = [];
            this.getTasks();                
        },
        updateCompleted() {
            
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
