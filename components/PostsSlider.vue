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
    this.postsWithComments = await Promise.all(
      this.allPosts.map(async (post) => {
        return {
          ...post,
          comments: await this.$http.$get(`/posts/${post.id}/comments`),
        }
      })
    )
  },
  data() {
    return {
      allPosts: [],
      postsWithComments: [],
      swiperOptions: {
        slidesPerView: 4,
        spaceBetween: 40,
        loop: true,
      },
    }
  },
  computed: {
    postsToShow() {
      return this.postsWithComments.length
        ? this.postsWithComments.slice(0, 10)
        : []
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

  .swiper-slide {
    min-height: 100%;
  }
}
</style>
