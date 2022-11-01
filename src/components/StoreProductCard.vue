<template>
  <article class="product-card">
    <img
        :src="imageUrl"
        :alt="`picture of ${title}`"
        class="product-card__image">
    <div class="product-card__title">{{ title }}</div>
    <div class="product-card__price">Price: {{ price }}$</div>
    <button
        @click="addNewItemToCart"
        class="product-card__button"
    >Add to cart
    </button>
  </article>
</template>

<script setup>
const emit = defineEmits(['addNewItemToCart']);
const {card} = defineProps({
  card: {
    type: Object,
    required: true,
  }
});

const {title, price, images: [imageUrl]} = card;

const addNewItemToCart = () => {
  emit('addNewItemToCart', card);
};
</script>

<style scoped lang="scss">
.product-card {
  display: flex;
  box-sizing: border-box;
  flex-direction: column;
  row-gap: .5rem;
  height: 100%;
  padding: 10px 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.25);

  &__image {
    display: block;
    width: 200px;
    height: 200px;
    border-radius: 8px;
  }

  &__title {
    font-weight: 700;
  }

  &__button {
    margin-top: auto;
  }
}
</style>