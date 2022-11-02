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
          <span>
            {{ numberOfCartPositions }}
          </span>
        </button>
      </nav>
    </header>
    <ul class="store__products-list product-list">
      <li
          v-for="product of products"
          class="product-list__item"
      >
        <store-product-card
            :card="product"
            class="product-list__product-card"
            @add-new-item-to-cart="addCartItemById"
        >
        </store-product-card>
      </li>
    </ul>
    <store-cart
        v-if="isOpenedCart"
        :cart-items="cartItems"
        @remove-cart-item-by-id="removeCartItemById"
        @close-cart="closeCart"
    >
    </store-cart>
  </div>

</template>

<script setup>
import {computed, onMounted, ref} from 'vue';
import StoreProductCard from './components/StoreProductCard.vue';
import StoreCart from './components/StoreCart.vue'

const products = ref([]);
const cartItems = ref({});
const isOpenedCart = ref(false);

const getProducts = async () => {
  const res = await fetch('https://dummyjson.com/products');
  return (await res.json()).products;
};

const numberOfCartPositions = computed(() => Object.keys(cartItems.value).length);

onMounted(async () => {
  products.value = await getProducts();
});

const addCartItemById = (productCard) => {
  if (cartItems.value[productCard.id]) {
    cartItems.value[productCard.id].quantity++;
  } else {
    cartItems.value[productCard.id] = {
      quantity: 1,
      productCard
    };
  }
};

const openCart = () => {
  isOpenedCart.value = true;
};

const closeCart = () => {
  isOpenedCart.value = false;
}

const removeCartItemById = (id) => {
  delete cartItems.value[id];
};
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
}

.nav-list {
  display: flex;
  min-width: 200px;
  height: min-content;
  justify-content: space-between;

  &__link {
    font-size: 1em;
    font-weight: 600;
    text-decoration: none;
    color: black;
  }
}
</style>
