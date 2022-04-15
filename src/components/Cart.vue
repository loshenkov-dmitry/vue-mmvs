<template>
  <div class="wrapper">
    <div class="cart-body">
      <div class="products" v-if="!loading">
        <Product
          v-for="product in products"
          :key="product.id"
          :product="product"
          @addToCart="addToCart"
          @removeFromCart="removeFromCart"
          @changeProductQty="changeProductQty"
        />
      </div>
      <Loader v-else />
    </div>
    <aside class="cart-aside">
      <div class="total">
        <TotalItems v-if="total.length > 0" :items="total" />
        <div v-else>Корзина пуста</div>
        <hr />
        <BtnOrder :disabled="cartEmpty" @showModal="showModal" />
      </div>
    </aside>

    <Modal title="Итог" v-show="modalShown" @closeModal="closeModal">
      <div slot="sum">{{ totalSum }} &#8381;</div>
    </Modal>
  </div>
</template>

<script>
import BtnOrder from "@/components/BtnOrder";
import Product from "@/components/Product";
import Modal from "@/components/Modal";
import TotalItems from "@/components/TotalItems";
import Loader from "@/components/Loader";
export default {
  data() {
    return {
      loading: true,
      cartEmpty: true,
      modalShown: false,
      products: [],
      total: [],
    };
  },

  mounted() {
    this.getProducts();
  },

  methods: {
    getProducts() {
      const URL = "https://fakestoreapi.com";
      fetch(`${URL}/products/`)
        .then((res) => res.json())
        .then((data) => {
          this.products = data;
          this.products.forEach((product) => {
            product.quantity = 0;
            product.price = Math.round(product.price);
            this.loading = false;
          });
        });
    },

    addToCart(id, qty) {
      qty = parseInt(qty);
      if (qty === 0) {
        return;
      }
      this.cartEmpty = false;
      let product = this.products.find((item) => {
        return id === item.id;
      });

      product.quantity = qty;

      let index = this.total.indexOf(product);

      if (index === -1) {
        this.total.push(product);
      } else {
        this.total.splice(index, 1, product);
      }
    },

    removeFromCart(id) {
      let product = this.products.find((item) => {
        return id === item.id;
      });
      product.quantity = 0;
      this.total = this.total.filter((item) => item.id !== product.id);
      if (this.total.length < 1) {
        this.cartEmpty = true;
      }
    },

    changeProductQty(id, qty) {
      let product = this.products.find((item) => {
        return id === item.id;
      });
      if (!qty) {
        this.quantity = 0;
      }
      if (qty > 99) {
        this.quantity = 99;
      }

      product.quantity = qty;
    },

    showModal() {
      this.modalShown = true;
    },

    closeModal() {
      this.modalShown = false;
    },
  },

  components: { Product, TotalItems, BtnOrder, Modal, Loader },

  computed: {
    totalSum: function () {
      let sum = [];

      for (let item of this.total) {
        sum.push(parseInt(item.quantity) * parseInt(item.price));
      }

      sum = sum.reduce((sum, item) => {
        return sum + item;
      }, 0);

      return sum;
    },
  },
};
</script>

<style lang="scss">
img {
  max-width: 100%;
  height: auto;
}
.wrapper {
  max-width: 1400px;
  padding: 0 15px;
  width: 100%;
  margin: 0 auto;
  display: flex;
}

.cart-body {
  width: 85%;
}

.cart-aside {
  width: calc(15% - 50px);
  position: relative;
  margin-left: 50px;
}

.products {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1rem;
  margin-bottom: 50px;
}

.total {
  position: sticky;
  top: 25px;
}

.btn {
  font-size: 20px;
  border-radius: 15px;
  padding: 10px 20px;
  cursor: pointer;
  transition: 0.2s;
}
</style>