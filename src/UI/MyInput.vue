<template>
  <div class="search">
    <label for="search" v-if="icon"
      ><img src="../assets/img/search.svg" alt="search" class="search__img"
    /></label>
    <input
      id="search"
      class="search__input"
      :type="type"
      :placeholder="place"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
  </div>
</template>

<script>
export default {
  name: "MyInput",
  props: ["modelValue", "icon", "type", "place"],
};
</script>

<style lang="scss" scoped>
.search {
  display: flex;
  align-items: center;
  gap: 10px;
  box-shadow: 0 1px 0 rgba(204, 204, 204, 0.3);
  border-bottom: 1px solid rgba(204, 204, 204, 0.25);
  padding: 7px;

  label {
    cursor: pointer;
  }

  &__input {
    width: 100%;

    &::placeholder {
      font-style: normal;
      font-weight: 400;
      font-size: 14px;

      color: rgba(0, 0, 0, 0.4);
    }
  }
}
</style>
