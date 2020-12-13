<template>
  <div class="posts-slider">
    <h2 class="posts-slider__title">Наши последние посты:</h2>
    <div class="posts-slider__swiper">
      <client-only>
        <Swiper ref="mySwiper" :options="swiperOptions" class="swiper">
          <SwiperSlide v-for="post in postsToShow" :key="post.id">
            <PostCard :post="post" />
          </SwiperSlide>
        </Swiper>
      </client-only>
    </div>
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
      swiperOptions: {
        slidesPerView: 4,
        spaceBetween: 40,
      },
    }
  },
  computed: {
    postsToShow() {
      return this.allPosts.length ? this.allPosts.slice(0, 10) : []
    },
  },
}
</script>

<style lang="scss">
.posts-slider {
  padding: 0 96px;
  max-width: 1200px;
  margin: auto;
  &__title {
    font-size: 3rem;
    margin-bottom: 40px;
  }
}
</style>
