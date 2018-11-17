<template>
    <ul>
        <li class="task">
            <button class="task-item"> task </button>
            <button class="delete red">
                 <font-awesome-icon class="icon" icon="times" />
            </button>
        </li>

    </ul>
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
        updateTasks() {
            axios.get('https://todo-list-memo.firebaseio.com/tasks.json')
                .then(response => {
                    const db_tasks = response.data;
                    for (const key in db_tasks) {
                       this.tasks.push(db_tasks[key].task);
                    }    
                })
                .catch(error => console.log(error));
                
        }
    },
    created () {
        axios.get('https://todo-list-memo.firebaseio.com/tasks.json')
            .then(response => {
                const db_tasks = response.data;
                    for (const key in db_tasks) {
                       this.tasks.push(db_tasks[key].task);
                    } 
            })
            .catch(error => console.log(error));
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
