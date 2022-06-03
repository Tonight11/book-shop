<template>
  <div class="popup">
    <div class="popup__shadow">
      <div class="popup__content">
        <slot></slot>
        <img
          class="popup__close"
          v-if="closable"
          src="../assets/img/x.svg"
          alt="close"
          @click="closeModal"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyDialog",
  props: ["closable"],
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
  },
};
</script>

<style lang="scss" scoped>
.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 20;

  &__shadow {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__content {
    max-width: 500px;
    width: 100%;
    padding: 15px;
    border-radius: 11px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    align-items: center;
    gap: 15px;
    background-color: white;

    position: relative;
  }

  &__close {
    position: absolute;
    top: 20px;
    right: 20px;
    width: 15px;
    height: 15px;
    cursor: pointer;
  }
}
</style>
