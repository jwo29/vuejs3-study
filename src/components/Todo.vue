<script setup lang="ts">
import { ref, computed, isProxy, toRaw } from 'vue'

const date = ref('')
// interface todo {
//     title: String           // 할 일
//     priority: Number        // 우선 순위
//     isCompleted: Boolean    // 완료 여부 
// }

// const todos = ref([ref<todo>()])

let id = 0
const text = ref('')
const hideCompleted = ref(false)

const todos = ref([
    {title: "Vue.js", id: id++, isCompleted: false},
    {title: "Spring Boot", id: id++, isCompleted: false}
])

const filterdTodos = computed(() => {
    return hideCompleted.value ? todos.value.filter((e) => !e.isCompleted) : todos.value
})

function addTodo() {
    todos.value.push({title: text.value, id: id++, isCompleted: false})
    text.value = ''
}

function deleteTodo(todo) {
    todos.value = todos.value.filter((e) => e.id != todo.id)
}

const deleteRow = (index: number) => {
    todos.value.splice(index, 1)
}


function onInput(e) {
  text.value = e.target.value
  console.log(e)
}

function updateCheck(e) {
    // todos.value[row].isCompleted = selection
    if (isProxy(e)) {
        const rawObject = {...e}
        console.log(rawObject)
    }
    // console.log(e.row)
    // console.log(todos.value[row])
}

</script>

<template>
    <div class="common-layout">
        <el-container>
            <el-header class="header">To do list</el-header>
            <el-container>
                <el-aside width="400px">
                    <el-calendar 
                        v-model="date"
                    />
                </el-aside>
                <el-main>
                    <el-input v-model="text" style="width: 70%;" placeholder="New todo" clearable/>
                    <!-- <input type="text" :value="text" @input="onInput"> -->
                    <el-button type="primary" @click="addTodo">Add</el-button>
                    <!-- <button @click="addTodo">Add todo</button> -->
                    
                    <!-- 할 일 목록 -->
                    <el-table 
                        :data="filterdTodos" 
                        height="250" 
                        style="width: 100%"
                        @select="updateCheck"
                    >
                        <el-table-column type="selection" label="Done" width="120" prop="isCompleted"/>
                        <el-table-column prop="title" label="Todo" width="300"/>
                        <el-table-column>
                            <template #default="scope">
                                <el-button 
                                    link
                                    type="primary"
                                    size="small"
                                    @click.prevent="deleteRow(scope.$index)"
                                >
                                    Remove
                                </el-button>
                            </template>
                        </el-table-column>
                    </el-table>

                    <!-- <ol>
                        <li v-for="todo in filterdTodos" :key="todo.id">
                            <input type="checkbox" v-model="todo.isCompleted">
                            <span :class="{ isCompleted: todo.isCompleted }">{{ todo.title }}</span>
                            <button @click="deleteTodo(todo)">X</button>
                        </li>
                    </ol> -->
                    <el-button @click="hideCompleted = !hideCompleted">
                        {{ hideCompleted ? '전체보기' : '완료한 항목 숨기기' }}
                    </el-button>

                </el-main>
            </el-container>
        </el-container>
    </div>
</template>

<style>

.isCompleted {
    text-decoration: line-through;
}
</style>