<template>
  <button class="btn-order" @click="$emit('showModal')" :disabled="cartEmpty">
    Заказать
  </button>
</template>

<script>
export default {
  props: {
    cartEmpty: Boolean,
  },
};
</script>

<style lang="scss">
.btn-order {
  background: rgb(12, 186, 55);
  border: none;
  color: #fff;
  font-size: 20px;
  border-radius: 15px;
  padding: 10px 20px;
  cursor: pointer;
  transition: 0.2s;
  &:disabled {
    cursor: not-allowed;
    filter: grayscale(1);
    opacity: 0.7;
  }
}
</style>