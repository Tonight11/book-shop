<template>
  <div class="cart__header">
    <div class="cart__close" @click="$emit('cart-close')">
      <img src="../assets/img/x.svg" alt="close" />
    </div>
    <div class="cart__content">
      <div class="cart__top">
        <div class="cart__text">Корзина</div>
        <img src="../assets/img/cart.svg" alt="cart" class="cart__icon" />
      </div>
      <div class="cart__row" v-if="cartItems.length">
        <transition-group name="cart-anim">
          <div
            class="card__column"
            v-for="(cart, index) in cartItems"
            :key="cart.title"
          >
            <div class="cart__item">
              <img :src="cart.book_image" alt="" class="cart__img" />
              <div class="cart__info">
                <div class="cart__title">
                  {{ cart.title }}
                </div>
                <div class="cart__counter">{{ cart.cartCount }} шт.</div>
                <div class="cart__price">
                  {{ cart.exactPrice * cart.cartCount }} руб
                </div>
              </div>
              <div class="cart__delete" @click="$emit('cart-delete', index)">
                <img src="../assets/img/x.svg" alt="delete" />
              </div>
            </div>
          </div>
        </transition-group>
      </div>
      <div class="cart__empty" v-else>Нет добавленых книг</div>
      <div class="cart__total">
        <div class="cart__total-price">{{ totalSum }} руб.</div>
        <my-button text="Купить!" @click="cartBuy"></my-button>
      </div>
    </div>
  </div>
</template>

<script>
import MyButton from "@/UI/MyButton";

export default {
  name: "TheCart",
  emits: ["cart-close", "bought"],
  props: ["cartItems", "counter"],
  components: {
    MyButton,
  },
  methods: {
    cartBuy() {
      if (!this.cartItems.length) {
        return;
      }
      this.$emit("bought", this.totalSum);
    },
  },
  computed: {
    totalSum() {
      let total = 0;
      this.cartItems.forEach((c) => {
        total += c.cartCount * c.exactPrice;
      });
      return total;
    },
  },
};
</script>

<style lang="scss" scoped>
.cart__header {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  z-index: 20;
  padding: 30px 50px;
  width: 400px;
  background: white;
  box-shadow: 0px 6px 15px -5px black;
  overflow-y: scroll;

  @media screen and (max-width: 768px) {
    width: 100%;
    box-shadow: none;
  }
}

.cart__close {
  position: absolute;
  top: 50px;
  right: 50px;
  cursor: pointer;
  img {
    width: 15px;
    height: 15px;
  }
}

.cart__content {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.cart__top {
  display: flex;
  align-items: center;
  gap: 10px;
  padding-bottom: 20px;
  margin-bottom: 50px;
}

.cart__text {
  font-size: 24px;
}

.cart__empty {
  opacity: 0.6;
}
.card__empty,
.cart__row {
  flex: 1;
  margin-bottom: 20px;
}

.card__column {
  margin-bottom: 20px;

  &:last-child {
    margin: 0;
  }
}

.cart__item {
  display: flex;
  gap: 10px;
}

.cart__info {
  flex: 1;
}

.cart__img {
  min-width: 100px;
  max-width: 100px;
  height: 100px;
  object-fit: cover;

  @media screen and (max-width: 768px) {
    height: 100%;
  }
}

.cart__title {
  font-size: 14px;
}

.cart__counter {
  font-size: 11px;
  opacity: 0.7;
}
.cart__delete {
  cursor: pointer;
}

.cart__total {
  margin-top: auto;
  text-align: center;
}

.cart-anim-item {
  display: inline-block;
  margin-right: 10px;
}
.cart-anim-enter-active,
.cart-anim-leave-active {
  transition: all 0.7s ease;
}
.cart-anim-enter-from,
.cart-anim-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>
