<template>
  <div class="home">
    <RepoCard msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import RepoCard from "@/components/RepoCard.vue";

export default {
  name: "home",
  components: {
    RepoCard
  }
};
</script>
