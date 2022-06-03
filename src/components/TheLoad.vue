<template>
  <div class="loader">
    <span></span>
    <span></span>
  </div>
</template>

<script>
export default {
  name: "TheLoad",
};
</script>

<style scoped>
.loader {
  position: relative;
  display: grid;
  place-items: center;
  width: 80px;
  height: 80px;
  margin: 0 auto;
}
span {
  position: absolute;
  border: 6px solid orangered;
  border-radius: 50%;
  animation: ripple 1.8s ease-out infinite;
}
span:nth-child(2) {
  animation-delay: -0.6s;
}
@keyframes ripple {
  from {
    opacity: 1;
    width: 0;
    height: 0;
  }
  to {
    opacity: 0;
    width: 100%;
    height: 100%;
  }
}
</style>
