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
export default {
    
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
