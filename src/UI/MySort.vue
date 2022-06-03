<template>
  <div class="sort-price">
    <div class="sort-price__text">Сортировать по цене</div>
    <input
      type="checkbox"
      id="check"
      :value="modelValue"
      name="yes"
      @change="sortByPrice"
    />
    <label for="check"
      ><img src="../assets/img/filter.svg" alt="" class="sort-price__img"
    /></label>
  </div>
</template>

<script>
export default {
  name: "MySort",
  props: {
    modelValue: Boolean,
  },
  methods: {
    sortByPrice(e) {
      if (e.target.checked) {
        this.$emit("update:modelValue", false);
      } else {
        this.$emit("update:modelValue", true);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.sort-price {
  display: flex;
  align-items: center;
  gap: 5px;

  &__img {
    cursor: pointer;
  }
}

input[type="checkbox"] {
  display: none;
}
</style>
