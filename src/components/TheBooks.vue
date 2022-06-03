<template>
  <div class="books">
    <div class="books__row" v-if="books.length">
      <transition-group name="list">
        <div v-for="book in books" :key="book.name" class="books__column">
          <div class="books__item">
            <div class="books__img">
              <img :src="'http://45.8.249.57' + book.coverUrl" alt="img" />
            </div>
            <div class="books__info">
              <div class="books__header">{{ book.name }}</div>
              <div class="books__author">{{ book.authorName }}</div>
              <div class="books__bot">
                <div class="books__price">
                  {{ book.price }} {{ book.countCard }} руб.
                </div>
                <my-button
                  text="В Корзину"
                  @click="$emit('add-cart', book)"
                ></my-button>
              </div>
            </div>
          </div>
        </div>
      </transition-group>
    </div>
    <div v-else-if="load">
      <the-load></the-load>
    </div>
    <div class="books__empty" v-else>Нет книг</div>
  </div>
</template>

<script>
import MyButton from "@/UI/MyButton";
import TheLoad from "@/components/TheLoad";
export default {
  name: "TheBooks",
  props: ["books", "load"],
  components: {
    MyButton,
    TheLoad,
  },
};
</script>

<style lang="scss" scoped>
.books {
  margin-top: 40px;
  height: 100%;

  &__row {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin: 0 -20px;
    row-gap: 40px;
  }

  &__empty {
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 1;
  }

  &__column {
    flex: 0 1 300px;
    padding: 0 20px;
  }

  &__item {
    display: flex;
    flex-direction: column;
    height: 100%;
    box-shadow: 0 0px 15px 0px rgba(0, 0, 0, 0.2);
    background-color: white;
    border-radius: 11px;
    overflow: hidden;
    transition: all 0.3s linear;
    &:hover {
      box-shadow: 0px 0px 15px 0px rgba(0, 0, 0, 0.6);
    }
  }

  &__img {
    margin-bottom: 20px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.5);

    img {
      width: 100%;
      height: 300px;
      object-fit: contain;
    }
  }

  &__info {
    padding: 10px;
    display: flex;
    flex-direction: column;
    height: 100%;
  }

  &__header {
    font-size: 24px;
    margin-bottom: 10px;
  }

  &__author {
    font-size: 16px;
    margin-bottom: 20px;
    flex: 1;
    opacity: 0.7;
  }

  &__bot {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
}

.list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.7s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-move {
  transition: transform 0.5s ease;
}
</style>
