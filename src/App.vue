<template>
  <div class="store">
    <header>
      <nav class="nav">
        <a
            class="nav__link"
            href="#"
        >
          Home
        </a>
        <a
            class="nav__link"
            href="#"
        >
          Product
        </a>
        <button @click="openCart">
          Cart
          <span>
            {{ numberOfCartPositions }}
          </span>
        </button>
      </nav>
    </header>
    <div class="product-list">
      <store-product-card
          v-for="product of products"
          :key="product.id"
          :card="product"
          @add-item-to-cart="addProductCardToCartById"
      />
    </div>
    <store-cart
        v-if="isOpenedCart"
        :cart-items="cartItems"
        @remove-cart-item-by-id="removeProductCardFromCartById"
        @close-cart="closeCart"
    />
  </div>

</template>

<script setup>
import {computed, onMounted, ref} from 'vue';
import StoreProductCard from './components/StoreProductCard.vue';
import StoreCart from './components/StoreCart.vue'

const products = ref([]);
const cartItems = ref({});
const isOpenedCart = ref(false);


const getAllProducts = async () => {
  const res = await fetch('https://dummyjson.com/products');
  return (await res.json()).products;
};

onMounted(async () => {
  products.value = await getAllProducts();
});


const addProductCardToCartById = (productCard) => {
  if (cartItems.value[productCard.id]) {
    cartItems.value[productCard.id].quantity++;
  } else {
    cartItems.value[productCard.id] = {
      quantity: 1,
      productCard
    };
  }
};

const removeProductCardFromCartById = (id) => {
  delete cartItems.value[id];
};


const openCart = () => {
  isOpenedCart.value = true;
};

const closeCart = () => {
  isOpenedCart.value = false;
}
const numberOfCartPositions = computed(() => Object.keys(cartItems.value).length);

</script>

<style scoped lang="scss">
.store {
  display: grid;
  row-gap: 2rem;
}

.product-list {
  display: grid;
  width: min-content;
  grid-template-columns: repeat(4, auto);
  justify-items: center;
  gap: 2rem;
}

.nav {
  display: flex;
  align-items: center;
  padding: 0 100px;
  justify-content: space-between;

  &__link {
    font-size: 1em;
    font-weight: 600;
    text-decoration: none;
    color: black;
  }
}
</style>
