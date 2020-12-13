<template>
  <nuxt-link :to="{}" class="post-card">
    <p class="post-card__title">{{ post.title }}</p>
    <p class="post-card__comment">{{ firstComment }}</p>
    <p class="post-card__user">Author ID: {{ post.userId }}</p>
  </nuxt-link>
</template>

<script>
export default {
  name: 'PostCard',
  props: {
    post: {
      type: Object,
      required: true,
    },
  },
  computed: {
    firstComment() {
      return this.post?.comments?.[0]?.body
    },
  },
}
</script>

<style lang="scss">
.post-card {
  border-radius: 8px;
  border: 1px solid lightgray;
  padding: 20px;
  display: grid;
  grid-gap: 16px;
  color: black;
  text-decoration: unset;
  height: 100%;
  &__title {
    font-weight: bold;
  }
}
</style>
