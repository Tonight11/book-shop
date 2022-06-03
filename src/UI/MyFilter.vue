<template>
  <select class="select" :value="modelValue" @change="changeOption">
    <option disabled selected value="">Категория</option>
    <option v-for="option in options" :key="option.id" :value="option.id">
      {{ option.name }}
    </option>
  </select>
</template>

<script>
export default {
  name: "MyFilter",
  props: {
    modelValue: String,
    options: Array,
  },
  methods: {
    changeOption(e) {
      this.$emit("update:modelValue", e.target.value);
    },
  },
};
</script>

<style scoped></style>
