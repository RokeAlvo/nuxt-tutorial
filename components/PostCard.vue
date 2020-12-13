<template>
  <div class="post-card">
    {{ post.id }}
  </div>
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

<style scoped></style>
