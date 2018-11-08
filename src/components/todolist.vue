<template>
    <div class="container">
        <div class="task-in">
            <input type="text"> 
            <input type="button" value="Añadir">    
        </div>        
    </div>
</template>

<script>
export default {
        
}
</script>

<style scoped>

.container {
    height: 30em;
    width: 40em;
    margin-top: 6em;
    background-color: #ffffff;
    border-radius: 1%;
    display: flex;
    justify-content: center;
}

.task-in {
    width: 100%;
    height: 4em;
    display: flex;
    align-items: center;

}

.container input[type=text] {
    width: 100%;
    height: 3em;
    margin: 10px;
    border-radius: 1%;
    border: 1px solid #b3b3b3;
}

.container input[type=button] {
    width: 20%;
    height: 3em;
    border: none;
    background-color: #000080;
    color: white;
    margin: 10px;
    
}


</style>
