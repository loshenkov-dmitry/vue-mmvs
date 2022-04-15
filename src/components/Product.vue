<template>
  <div class="product" :class="{ 'product-bordered': quantity > 0 }">
    <img :src="product.image" :alt="product.name" />
    <h3 v-text="product.title"></h3>
    <p v-text="product.description"></p>
    <h4>{{ product.price }} &#8381;</h4>

    <div class="product_btns">
      <input
        type="number"
        min="0"
        max="99"
        v-model.number="quantity"
        value="0"
      />
      <button
        class="product_btn product_btn--add"
        @click="$emit('addToCart', product.id, quantity)"
      >
        Добавить
      </button>
      <button
        class="product_btn product_btn--remove"
        @click="
          $emit('removeFromCart', product.id);
          quantity = 0;
        "
      >
        Удалить
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quantity: 0,
    };
  },
  props: {
    product: Object,
  },

  watch: {
    quantity: function (val) {
      if (!val) {
        this.quantity = 0;
      }
      if (val > 99) {
        this.quantity = 99;
      }
    },
  },
};
</script>

<style lang="scss">
.product {
  display: flex;
  flex-direction: column;

  padding: 30px;
  border-radius: 10px;
  box-shadow: 4px 4px 16px 6px rgba(34, 60, 80, 0.2);
  img {
    height: 400px;
    object-fit: contain;
  }
  &-bordered {
    position: relative;
    z-index: 1;
    &::after {
      content: "";
      display: block;
      width: 100%;
      height: 100%;
      border: 2px solid rgb(12, 186, 55);
      position: absolute;
      top: -1px;
      left: -1px;
      border-radius: 10px;
      z-index: -1;
    }
  }
}

.product_btns {
  margin-top: auto;

  input {
    padding: 4px 8px;
    margin-bottom: 30px;
    border-radius: 5px;
    border-color: #2c3e50;
  }
}

.product_btn {
  cursor: pointer;
  font-size: 15px;
  padding: 4px 8px;
  border-radius: 5px;
  margin-left: 5px;
  border: none;
  color: #fff;
  &:first-child {
    margin: 0;
  }

  &--add {
    background: #d68426;
  }

  &--remove {
    background: #c72121;
  }
}
</style>