<template>
    <div>
        <h1>issue list</h1>
        <el-button type="success" @click="getIssues()">Get issue</el-button>
        <el-row :gutter="12">

        <IssueItem :issue=issue v-for="( issue, index ) in issues" :key="issue.id" @close="closeIssue(index)"/>
    </el-row>
    </div>
</template>

<script>
    import axios from 'axios';
    import IssueItem from '@/components/IssueItem';


    const client = axios.create({  //--1
        baseURL: `${process.env.VUE_APP_GITHUB_ENDPOINT}`,
        headers: { //--3
            'Accept': 'application/vnd.github.v3+json',
            'Content-Type':'application/json',
            'Authorization':`token ${process.env.VUE_APP_GITHUB_TOKEN}`
        },
    })

    export default{
        name: 'IssueList',

        components:{
            IssueItem,
        },

        data(){
            return{
                issues: [],
            }
        },

        methods:{
            getIssues(){
                client.get('/issues')
                .then((res) => {
                this.issues = res.data;
                })
                // axios.get('https://api.github.com/repos/diveintocode-corp/vue_seriese_api/issues', {
                //     headers: {
                //         'Accept': 'application/vnd.github.v3+json',
                //         'Content-Type':'application/json',
                //         },
                //     }
                //     )
                //     .then((res) => {
                //     this.issues = res.data;
                // })
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
        },

        created(){
            this.getIssues();
        }
    }
</script>