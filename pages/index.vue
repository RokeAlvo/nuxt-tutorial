<template>
  <div class="home-page">
    <Billboard class="home-page__block" />
    <PostsSlider class="home-page__block" />
  </div>
</template>

<script>
import Billboard from '~/components/Billboard'
import PostsSlider from '~/components/PostsSlider'

export default {
  components: { PostsSlider, Billboard },
}
</script>

<style lang="scss">
.home-page {
  &__block {
    margin-bottom: 72px;
  }
}
</style>
