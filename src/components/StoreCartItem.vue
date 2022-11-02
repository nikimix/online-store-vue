<template>
  <article class="cart-item">
    <img
        :src="imageUrl"
        :alt="`picture of ${title}`"
        class="cart-item__image"
    >
    <div class="cart-item__text-content">
      <div class="cart-item__title">{{ title }}</div>
      <div class="cart-item__price">Price: {{ price }}$</div>
      <div class="cart-item__quantity">Quantity: {{ quantity }}</div>
      <div class="cart-item__price">Price: {{ totalPrice }}$</div>
    </div>
    <div class="cart-item__controls cart-item-controls">
      <button
          class="cart-item__button"
          @click="incrementItem"
      >
        +
      </button>
      <button
          class="cart-item__button"
          @click="decrementItem"
      >
        -
      </button>
    </div>
  </article>
</template>

<script setup>
import {computed, toRef} from 'vue';

const emit = defineEmits(['removeCartItemById']);

const {cartItem} = defineProps({
  cartItem: {
    type: Object,
    required: true,
  }
});

const {productCard: {id, title, price, images: [imageUrl]}} = cartItem;

const quantity = toRef(cartItem, 'quantity');

const totalPrice = computed(() => (quantity.value * price))

const incrementItem = () => {
  quantity.value++;
};

const decrementItem = () => {
  if (quantity.value > 1) {
    quantity.value--;
  } else {
    emit('removeCartItemById', id);
  }
};
</script>

<style scoped lang="scss">
@import '../main.scss';

.cart-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  column-gap: 2rem;

  &__image {
    width: 100px;
    height: 100px;
    @include box-shadow-1;
  }

  &__title {
    font-weight: 600;
  }
}

.cart-item-controls {
  display: grid;
  row-gap: 1rem;
  align-items: center;
}
</style>