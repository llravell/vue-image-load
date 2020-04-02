<template>
  <div id="app" class="container">
    <ImageUpload />
  </div>
</template>

<script>
const ImageUpload = () => import('./components/ImageUpload');

export default {
  name: 'App',
  components: {
    ImageUpload,
  }
}
</script>

<style scoped>
.container {
    padding: 0;
    width: 100%;
    height: 100vh;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>
