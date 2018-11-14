<template>
    <div class="container">
        <h1>Lista de tareas</h1>
        <div class="task-in">
            <input type="text" placeholder="Nueva tarea"> 
            <button class="button blue">
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
        <tasks></tasks>
    </div>
</template>

<script>

import tasks from './task.vue'

export default {
        components: {
            tasks
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
