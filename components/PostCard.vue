<template>
  <nuxt-link :to="{}" class="post-card">
    <p class="post-card__title">{{ post.title }}</p>
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

  &__title {
    font-weight: bold;
  }
}
</style>
