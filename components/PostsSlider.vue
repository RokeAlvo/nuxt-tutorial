<template>
  <div class="posts-slider">
    <PostCard v-for="post in postsToShow" :key="post.id" :post="post" />
  </div>
</template>

<script>
import PostCard from '~/components/PostCard'
export default {
  name: 'PostsSlider',
  components: { PostCard },
  async fetch() {
    this.allPosts = await this.$http.$get('/posts')
  },
  data() {
    return {
      allPosts: {
        type: Array,
        default: () => [],
      },
    }
  },
  computed: {
    postsToShow() {
      return this.allPosts.slice(0, 10)
    },
  },
}
</script>

<style lang="scss"></style>
