<template>
  <article class="cart-item">
    <img
        :src="imageUrl"
        :alt="`picture of ${title}`"
        class="cart-item__image"
    >
    <div>
      <div class="cart-item__title">{{ title }}</div>
      <div>Price: {{ price }}$</div>
      <div>Quantity: {{ quantity }}</div>
      <div>Price: {{ totalPrice }}$</div>
    </div>
    <div class="cart-item-controls">
      <button @click="incrementItem">+</button>
      <button @click="decrementItem">-</button>
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
@use '../main.scss';

.cart-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  column-gap: 2rem;

  &__image {
    width: 100px;
    height: 100px;
    @include main.box-shadow-1;
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