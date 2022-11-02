<template>
  <article class="product-card">
    <img
        :src="imageUrl"
        :alt="`picture of ${title}`"
        class="product-card__image"
    >
    <div class="product-card__title">{{ title }}</div>
    <div class="product-card__price">Price: {{ price }}$</div>
    <button
        class="product-card__button"
        @click="addItemToCart"
    >
      Add to cart
    </button>
  </article>
</template>

<script setup>
const emit = defineEmits(['addItemToCart']);
const {card} = defineProps({
  card: {
    type: Object,
    required: true,
  }
});

const {title, price, images: [imageUrl]} = card;

const addItemToCart = () => {
  emit('addItemToCart', card);
};
</script>

<style scoped lang="scss">
@use '../main.scss';

.product-card {
  display: flex;
  box-sizing: border-box;
  flex-direction: column;
  row-gap: .5rem;
  height: 100%;
  padding: 10px 20px;
  border-radius: 8px;
  @include main.box-shadow-1;

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
    transition: .3s;
    &:hover {
      @include main.box-shadow-1;
    }
  }
}
</style>