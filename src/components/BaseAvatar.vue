<template>
  <div>
    <img :src="avatar">
  </div>
</template>

<script>
export default {
  props: {
    avatar: String
  }
};
</script>

<style scoped>
img {
  border-radius: 50%;
  border: 4px solid #65b787;
}
</style>
