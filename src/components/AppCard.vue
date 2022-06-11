<template>
  <li>
    <img :src="image.url" :alt="image.alt"/>
  </li>

</template>

<script>

export default {
  name: "AppCard",
  props: {
    image: {
      type: Object,
      default() {
        return { url: '', alt: '' }
      }
    }
  }
}
</script>

<style scoped>
@keyframes fadeIn {
  0% {
    transform: translateY(-10px) scale(0.95);
    opacity: 0;
  }

  100%{
    transform: translateY(0) scale(1);
    opacity: 1;
  }
}
img {
  width: 100%;
  max-height: 100%;
  display: block;
  border: 1px solid transparent;
  border-radius: 4px;
  overflow: hidden;

  animation-name: fadeIn;
  animation-duration: 0.5s;
  animation-timing-function: ease-in-out;
  animation-fill-mode: both;
}
</style>
