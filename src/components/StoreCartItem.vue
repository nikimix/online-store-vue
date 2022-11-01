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
      <div>Quantity: {{ quantity }}</div>
      <div>Price: {{ sum }}$</div>
    </div>
    <div class="cart-item__controls cart-item-controls">
      <button
          @click="addItem"
          class="cart-item__button"
      >
        +
      </button>
      <button
          @click="removeItem"
          class="cart-item__button"
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

const sum = computed(() => (quantity.value * price))

const addItem = () => {
  quantity.value++;
};

const removeItem = () => {
  if (quantity.value > 1) {
    quantity.value--;
  } else {
    emit('removeCartItemById', id);
  }
};

</script>
<style scoped lang="scss">
.cart-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  column-gap: 2rem;

  &__image {
    width: 100px;
    height: 100px;
    box-shadow: 1px 1px 6px 1px black, -1px -1px 6px 1px black;
  }
}

.cart-item-controls {
  display: grid;
  row-gap: 1rem;
  align-items: center;
}
</style>