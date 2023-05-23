<template>
    <div>
        <h1>TodoList</h1>
        <form @submit.prevent="addTodo()">
            <el-input v-model="todo" type="text"></el-input>
        </form>

        <el-row :gutter="12">
            <TodoItem :todo=todo v-for="( todo, index ) in todos" :key="index" @close="removeTodo(index)"/>
        </el-row>
    </div>
</template>

<script>
    import TodoItem from '@/components/TodoItem';
    export default{
        name: 'TodoList',

        components: {
            TodoItem
        },
        data(){
            return {
                todo: '',

                todos: [],
            }
        },

        methods:{
            addTodo(){
                this.todos.push(this.todo);
                this.todo= '';
            },

            removeTodo(index){
                this.todos.splice(index,1);
            }
        }
    }
</script>