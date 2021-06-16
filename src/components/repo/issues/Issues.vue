<template>
    
    <div v-if="issues.length > 0">

        <button v-on:click = "showIssues = !showIssues">{{ showIssues ? 'Hide' : 'Show'}} issues</button>

        <div v-if = "showIssues">
            
            <div v-for="i in issues" :key="i.id">
                
                <h3>{{i.title}}</h3>

                <a :href="i.url">Go to issues</a>

                <IssueComments :owner="owner" :repo="repo" :issueNumber="i.number"/>

            </div>

        </div>

    </div>

</template>

<script>

import { octokitMixin } from "/src/mixins/octokitMixin";

import IssueComments from "./Comments.vue";

export default {
    name : "RepoIssues",
    components : {
        IssueComments,
    },
    props : {
        owner: {
        type: String,
        required: true,
        },
        repo: {
        type: String,
        required: true,
        },
    },
    mixins : [octokitMixin],
    data(){
        return {
            issues : [],
            showIssues : false
        }
    },
    methods : {
        async getRepoIssues (owner, repo){
            if(
                typeof owner !== "string" ||
                typeof repo !== "string"
            ){
                return;
            }

            const octokit = this.createOctokitClient();

            const {data : issues} = await

                octokit.issues.listForRepo({
                    owner,
                    repo
                })

            this.issues = issues;
        }
    },
    watch : {
        owner : {
            handler(val){
                this.getRepoIssues(val, this.repo);
            },
        },
        repo : {
            handler(val){
                this.getRepoIssues(this.owner, val);
            },
        },
    },
    created(){
        this.getRepoIssues(this.owner, this.repo);
    }
}

</script>
