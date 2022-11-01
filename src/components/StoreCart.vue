<template>
  <dialog
      open
      class="store__cart cart"
  >
    <div
        v-show="isCartEmpty"
        class="cart__content-wrapper"
    >
      <ul class="cart__list cart-list">
        <li
            v-for="(cartItem, key) in cartItems"
            :key="key"
            class="cart-list__item"
        >
          <store-cart-item
              :cart-item="cartItem"
              class="cart-list__cart-item"
              @remove-cart-item-by-id="removeCartItemById"
          >
          </store-cart-item>
        </li>
      </ul>
      <div class="cart__total-sum">Total sum of products: {{ totalSum }}$</div>
    </div>
    <div
        v-show="!isCartEmpty"
        class="cart__message-empty-cart"
    >
      There are no products.
    </div>
    <button
        class="cart__close-button"
        @click="closeCart"
    >
      X
    </button>
  </dialog>
</template>

<script setup>
import {computed} from "vue";
import StoreCartItem from './StoreCartItem.vue';

const emit = defineEmits(['removeCartItemById', 'closeCart'])

const {cartItems} = defineProps({
  cartItems: {
    type: Object,
    required: true,
  },

})

const totalSum = computed(() => {
  return Object.values(cartItems).reduce((acc, {
    quantity,
    productCard
  }) => acc += quantity * productCard.price, 0);
});

const isCartEmpty = computed(() => Object.keys(cartItems).length);

const closeCart = () => {
  emit('closeCart');
};

const removeCartItemById = (id) => {
  emit('removeCartItemById', id);
}

</script>

<style scoped lang="scss">

</style>