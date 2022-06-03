<script setup>
import { reactive, ref } from 'vue';

let id = 0;
let list = ref([
    {id : id++, name : "Example Task 1", completed : false},
    {id : id++, name : "Example Task 2", completed : false},
    {id : id++, name : "Example Task 4", completed : false},
    {id : id++, name : "Example Task 5", completed : false}
]);

const props = defineProps({
    listName : String
});

let newTaskName = ref("");

function addTask(){
    list.value.push({
        id : id++,
        name : newTaskName.value,
        completed : false,
        deleted : false
    });
    newTaskName.value = "";
}

function deleteTask(task){
    list.value = list.value.filter(item=>item.id != task.id);
}

</script>

<template>
<main>
    <h1 class="title">{{ listName }}</h1>
    <ul class="todoList">
        <li
        :class="item.completed ? 'listCompleted' : 'listNormal'"
        v-for="item in list"
        :key="item.id"
        >
        <label>
        {{ item.name }} <span v-show="item.completed">✔️</span>
        <input
        type="checkbox"
        class="completeCheck"
        v-model="item.completed"
        >
        </label>
        <button
        class="deleteButton"
        v-show="!item.completed"
        @click="deleteTask(item)"
        >
        ❌
        </button>
        </li>
    </ul>
    <section class="inputSection">
        <input
        class="itemInput"
        type="text"
        placeholder="New item"
        v-model="newTaskName"
        >
        <button
        class="addButton"
        v-show="newTaskName.length"
        @click="addTask"
        >
        ➕
        </button>
    </section>
</main>
</template>

<style>
main{
    display: flex;
    flex-flow: column wrap;
    align-items: center;
    user-select: none;
    border: 10px double #FFC8DD;
    border-radius: 50%;
    padding: 5rem;
    background: #FFAFCC;
}
.todoList{
    list-style-position:inside;
}
.title{
    margin-bottom: 1rem;
}
.inputSection{
    display: flex;
    flex-flow: column wrap;
    align-items: center;
    gap: .5rem;
    margin-top: 1rem;
}
.itemInput{
    padding: .5rem;
    border-radius: 12px;
    background: #f093c6;
    color: #fff;
    font-weight: bolder;
    font-size: 1rem;
    text-align: center;
}
.addButton{
    padding: .5rem;
    background: #BDE0FE;
    border-radius: 12px;
    width: 150px;
    transition: all .1s;
}
.deleteButton{
    border-radius: 50%;
    background: #fff;
    transition: all .5s;
}
.deleteButton:hover{
    cursor: pointer;
    background: red;
    transform: scale(1.1);
    transform: rotate(360deg);
}
.addButton:hover{
    cursor: pointer;
    background: #A2D2FF;
    transform: scale(1.1);
}
.listNormal{
    list-style-type: symbols(cyclic "😺");
}
.listCompleted{
    list-style-type: symbols(cyclic "😻");
    text-decoration: line-through;
    color: #BDE0FE;
}
.completeCheck{
    display: none;
}
</style>