<template>
  <div v-if="cart" class="btn-fixed">
    <button class="btn btn-cart" @click="$emit('show-cart')">{{ text }}</button>
    <span class="cart-count">{{ total.length }}</span>
  </div>
  <button v-else class="books__btn btn" @click.prevent="animating">
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "MyButton",
  props: ["text", "cart", "total"],
  methods: {
    animating(e) {
      e.target.classList.remove("animate");

      e.target.classList.add("animate");
      setTimeout(function () {
        e.target.classList.remove("animate");
      }, 700);
    },
  },
};
</script>

<style lang="scss" scoped>
.btn-fixed {
  position: fixed;
  bottom: 10%;
  right: 10%;
  z-index: 10;
}
.btn-cart {
  background: black;
  color: white;
}
.cart-count {
  position: absolute;
  top: -20px;
  right: -20px;
  padding: 5px;
  border-radius: 50%;
  background: #589254;
  color: white;
  min-width: 30px;
  min-height: 30px;
  text-align: center;
}

.books__btn {
  border: 1px solid #589254;
  display: inline-block;
  -webkit-appearance: none;
  appearance: none;
  position: relative;
  transition: all 0.3s ease;

  &:hover {
    box-shadow: 0px 6px 15px -5px #589254;
  }
}
</style>
