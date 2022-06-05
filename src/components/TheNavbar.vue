<template>
  <header class="header">
    <div class="container">
      <div class="header__inner">
        <div class="header__logo">
          <img
            class="header__logo-img"
            src="../assets/img/book.svg"
            alt="logo"
          />
          <span class="header__logo-text">Магазин книг</span>
        </div>
        <div class="header__right">
          <div class="header__balance">Баланс: {{ balance }} руб.</div>
          <my-button
            text="Пополнить баланс!"
            @click="$emit('add-balance')"
          ></my-button>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import MyButton from "@/UI/MyButton";
export default {
  name: "TheNavbar",
  emits: ["add-balance"],
  props: ["balance"],
  components: {
    MyButton,
  },
};
</script>

<style lang="scss" scoped>
.header {
  padding: 20px 0;
  margin-bottom: 50px;
  border-bottom: 1px solid rgba(204, 204, 204, 0.25);

  &__inner {
    display: flex;
    align-items: center;
    justify-content: space-between;

    @media screen and (max-width: 587px) {
      flex-direction: column;
      gap: 20px;
    }
  }

  &__logo {
    display: flex;
    align-items: center;
    gap: 15px;
  }

  &__logo-text {
    font-weight: 400;
    font-size: 32px;
    font-style: normal;
  }

  &__right {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  &__balance {
    font-style: normal;
    font-weight: 400;
    font-size: 20px;
  }
}
</style>
