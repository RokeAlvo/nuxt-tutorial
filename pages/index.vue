<template>
  <div class="home-page">
    <Billboard />
    <PostsSlider />
  </div>
</template>

<script>
import Billboard from '~/components/Billboard'
import PostsSlider from '~/components/PostsSlider'

export default {
  components: { PostsSlider, Billboard },
}
</script>

<style lang="scss"></style>
