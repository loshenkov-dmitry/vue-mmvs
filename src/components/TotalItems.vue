<template>
  <div class="total-items">
    <div class="total-item" v-for="item in items" :key="item.id">
      <span> {{ item.title }}</span>
      <strong v-if="item.quantity > 1"> ({{ item.quantity }}) шт</strong>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: Array,
  },
};
</script>

<style lang="scss">
.total-items {
  display: grid;
  grid-gap: 10px;
  margin-bottom: 15px;
}

.total-item {
  text-align: left;
}
</style>