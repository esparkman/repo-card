<template>
  <div>
    <h1>{{ msg  }}</h1>
    <div class="cards">
      <div class="repo-card" v-for="(repo, index) in repos" :key="index.id">
        <BaseAvatar :avatar="user.avatarUrl" />
        <h2>{{ user.name }}</h2>
        <h2><a class="repo-link" :href="repo.node.url">{{ repo.node.name }}</a></h2>
        <span>{{ repo.node.description ? repo.node.description : 'No Description Available' }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RepoCard",
  props: {
    msg: String
  },
  data() {
    return {
      user: {},
      repos: []
    };
  },
  methods: {
    async getUser() {
      try {
        const res = await axios({
          url: "https://api.github.com/graphql",
          method: "post",
          headers: {
            Authorization: `bearer ${process.env.VUE_APP_GH_PERSONAL_TOKEN}`
          },
          data: {
            query: `
              query {
                viewer {
                  login
                  name
                  avatarUrl
                }
              }
            `
          }
        });
        this.user = res.data.data.viewer;
      } catch (e) {
        console.log("err", e);
      }
    },
    async getRepos() {
      try {
        const res = await axios({
          url: "https://api.github.com/graphql",
          method: "post",
          headers: {
            Authorization: `bearer ${process.env.VUE_APP_GH_PERSONAL_TOKEN}`
          },
          data: {
            query: `
              query {
                viewer {
                  repositories(first: 50) {
                    edges {
                      node {
                        id
                        name
                        url
                        description
                      }
                    }
                  }
                }
              }
            `
          }
        });
        this.repos = res.data.data.viewer.repositories.edges;
      } catch (e) {
        console.log("err", e);
      }
    }
  },
  created() {
    this.getUser();
    this.getRepos();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.cards {
  display: flex;
  justify-content: center;
  flex-direction: row;
  flex-wrap: wrap;
}
.repo-card {
  padding: 20px;
  margin: 24px;
  width: 500px;
  transition: all 0.2s linear;
  cursor: pointer;
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2), 0 1px 15px 0 rgba(0, 0, 0, 0.19);
}
.repo-card:hover {
  transform: scale(1.01);
  box-shadow: 0 3px 12px 0 rgba(0, 0, 0, 0.2), 0 1px 15px 0 rgba(0, 0, 0, 0.19);
}
.repo-card > .title {
  margin: 0;
}

.repo-link {
  color: black;
  text-decoration: none;
  font-weight: 100;
}
</style>
