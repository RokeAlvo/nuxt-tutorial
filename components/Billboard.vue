<template>
  <div class="billboard">
    <img src="/girl-4898696_1920.jpg" alt="girl" class="billboard__image" />
    <h1 class="billboard__title">Очень, очень важный заголовок</h1>
  </div>
</template>

<script>
export default {
  name: 'Billboard',
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
