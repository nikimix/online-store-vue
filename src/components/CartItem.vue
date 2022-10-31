<template>
  <article class="cart-item">
    <img src="" alt="">
    <div class="cart-item__title">{{ title }}</div>
    <div class="cart-item__price">Price: {{ price }}$</div>
    <div class="cart-item__description">{{ description }}</div>
    <div>Quantity: {{ quantity }}</div>
    <div>Common price: {{sum}}$</div>
    <button @click="addItem" class="cart-item__button">+</button>
    <button @click="removeItem" class="cart-item__button">-</button>
  </article>
</template>
<script setup>
import { computed, toRef } from 'vue';

const emit = defineEmits(['removeFromCart']);
const { cartItem } = defineProps({
  cartItem: {
    type: Object,
    required: true,
  }
});
const { product: { title, price, description } } = cartItem.value;
const quantity = toRef(cartItem.value, 'quantity');

const sum = computed(() => (quantity.value * price))

const addItem = () => {
  quantity.value++;
};

const removeItem = () => {
  if (quantity.value > 1) {
    quantity.value--;
  } else {
    emit('removeFromCart', cartItem);
  }
};

</script>
<style scoped lang="scss"></style>