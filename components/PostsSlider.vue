<template>
  <div class="posts-slider">slider</div>
</template>

<script>
export default {
  name: 'PostsSlider',
  async fetch() {
    this.allPosts = await this.$http.$get(
      'https://jsonplaceholder.typicode.com/posts'
    )
  },
  data() {
    return {
      allPosts: {
        type: Array,
        default: () => [],
      },
    }
  },
}
</script>

<style lang="scss"></style>
