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
  top: -10px;
  right: -5px;
  padding: 5px;
  border-radius: 50%;
  background: orangered;
  color: white;
}

.books__btn {
  border: 1px solid orangered;
  display: inline-block;
  -webkit-appearance: none;
  appearance: none;
  position: relative;
  transition: all 0.3s ease;

  &:hover {
    box-shadow: 0px 6px 15px -5px orangered;
  }
}
</style>
