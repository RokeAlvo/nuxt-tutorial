<template>
  <div class="billboard">
    <img src="/girl-4898696_1920.jpg" alt="girl" class="billboard__image" />
    <h1 class="billboard__title">{{ title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Billboard',
  async fetch() {
    // получили откуда-то из api
    this.title = await Promise.resolve('Очень, очень важный заголовок')
  },
  data() {
    return {
      title: '',
    }
  },
}
</script>

<style lang="scss">
.billboard {
  width: 100%;
  height: 100vh;

  &__image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  &__title {
    font-size: 6rem;
    color: #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    width: 80%;
  }
}
</style>
