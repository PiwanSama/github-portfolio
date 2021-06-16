<template>
  <div>
    <div v-if="comments.length > 0">
      <h4>Comments</h4>

      <div v-for="c of comments" :key="c.id">
        {{ c.user && c.login }} - {{ c.body }}
      </div>
    </div>
  </div>
</template>

<script>
import { octokitMixin } from "/src/mixins/octokitMixin";

export default {
  name: "Comments",
  data() {
    return {
      owner: "",
      repo: "",
      issueNumber: 0,
      comments: [],
    };
  },
  methods: {
    async getIssuesComments(owner, repo, issueNumber) {
      if (
        typeof owner !== "string" ||
        typeof repo !== "string" ||
        typeof issueNumber !== "number"
      ) {
        return;
      }
      const octokit = this.createOctokitClient();

      const { data: comments } = await octokit.issues.listComments({
        owner,
        repo,
        issue_number: issueNumber,
      });

      this.comments = comments;
    },
  },
  props: {
    owner: {
      type: String,
      required: true,
    },
    repo: {
      type: String,
      required: true,
    },
    issueNumber: {
      type: Number,
      required: true,
    },
  },
  mixins: [octokitMixin],
};
</script>