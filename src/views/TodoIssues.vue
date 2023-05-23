<template>
    <div>
      <h1>todo issues</h1>
      
      <form @submit.prevent="addTodo()">
        <el-input placeholder="todo" v-model="todochild"></el-input>
      </form>
      <el-row :gutter="12">
       
        <TodoItem :todo=todo v-for="( todo, index ) in todos" :key="index" @close="removeTodo(index)"/>
        
        <IssueItem :issue=issue v-for="( issue, index ) in issues" :key="issue.id" @close="closeIssue(index)"/>
      </el-row>
    </div>
  </template>
  
  <script>
  import axios from 'axios';

  import TodoItem from '@/components/TodoItem';
  import IssueItem from '@/components/IssueItem';
  
  const client = axios.create({
    baseURL: `${process.env.VUE_APP_GITHUB_ENDPOINT}`,
    headers: {
      'Authorization': `token ${process.env.VUE_APP_GITHUB_TOKEN}`,
      'Accept': 'application/vnd.github.v3+json',
      'Content-Type':'application/json',
    },
  })
  
  export default {
    name: 'TodosIssues',

    components: {
        TodoItem,
        IssueItem
    },

    data () {
      return {
        todochild: '',
        todos: [],
        issues: []
      }
    },
    methods: {
      
      addTodo(){
        this.todos.push(this.todochild);
        this.todochild= '';
      },
      removeTodo(index){
        this.todos.splice(index, 1);
      },
      
      closeIssue(index){
        const target = this.issues[index];
        client.patch(`/issues/${target.number}`,
            {
              state: "closed"
            },
          )
          .then(() => {
           this.issues.splice(index, 1);
        })
      },
      getIssues() {
        client.get('issues')
          .then((res) => {
            this.issues = res.data
        })
      }
    },
    created() {
      this.getIssues();
    }
  }
  </script>