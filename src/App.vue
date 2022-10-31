<template>
  <div class="store">
    <header class="store__header header">
      <nav class="header__nav nav">
        <ul class="nav__list nav-list">
          <li class="nav-list__item">
            <a class="nav-list__link"
               href="#"
            >
              Home
            </a>
          </li>
          <li class="nav-list__item">
            <a class="nav-list__link"
               href="#"
            >
              Product
            </a>
          </li>
        </ul>
        <button @click="openCart">
          Cart
        </button>
      </nav>
    </header>
    <ul class="store__products-list product-list">
      <li
          v-for="product of products"
          class="product-list__item"
      >
        <product-card
            :card="product"
            @add-to-cart="addToCart"
            class="product-list__card"
        >
        </product-card>
      </li>
    </ul>
    <dialog
        :open="isOpenCart"
    >
      <div v-show="cartItems.length">
        <ul>
          <li
              v-for="cartItem of cartItems"
              :key="cartItem.value.product.id"
          >
            <cart-item
                :cart-item="cartItem"
                @remove-from-cart="removeFromCart"
            >
            </cart-item>
          </li>
        </ul>
        <div>Total sum of all products: {{ totalSum }}$</div>
      </div>
      <div v-show="!cartItems.length"> There are no products</div>
      <button @click="closeCart">X</button>
    </dialog>
  </div>

</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import ProductCard from './components/ProductCard.vue';
import CartItem from './components/CartItem.vue';

const products = ref([]);
const cartItems = ref([]);
const isOpenCart = ref(false);

const totalSum = computed(() =>
    cartItems.value.reduce((acc, { value: { quantity, product } }) => acc + quantity * product.price, 0)
);

const getProducts = async () => {
  const res = await fetch('https://dummyjson.com/products');
  return (await res.json()).products;
};

onMounted(async () => {
  products.value = await getProducts();
});

const addToCart = (product) => {
  const cartItem = cartItems.value.find(({ value }) => value.product === product);
  if (cartItem) {
    cartItem.value.quantity++;
  } else {
    cartItems.value.unshift(ref({ quantity: 1, product }));
  }
};

const openCart = () => {
  isOpenCart.value = true;
};

const closeCart = () => {
  isOpenCart.value = false;
};

const removeFromCart = (cartItem) => {
  const index = cartItems.value.findIndex(({ value }) => value === cartItem.value);
  cartItems.value.splice(index, 1);

  console.log(cartItems.value);
};
</script>

<style scoped lang="scss">

.store {
  display: grid;
  row-gap: 2rem;
}

.product-list {
  display: grid;
  margin: 0;
  padding: 0;
  grid-template-columns: repeat(4, auto);
  gap: 2rem;
}

.nav {
  display: flex;
  align-items: center;
  padding: 0 100px;
  justify-content: space-between;
}

.nav-list {
  display: flex;
  min-width: 200px;
  height: min-content;
  margin: 0;
  padding: 0;
  justify-content: space-between;
  list-style: none;

  &__link {
    font-size: 1em;
    font-weight: 600;
    text-decoration: none;
    color: black;
  }
}
</style>
