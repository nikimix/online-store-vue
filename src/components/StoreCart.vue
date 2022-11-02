<template>
  <dialog
      ref="dialog"
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
        class="cart__button-close-cart"
        @click="closeCart"
    >
    </button>
  </dialog>
</template>

<script setup>
import {computed, nextTick, ref} from "vue";
import StoreCartItem from './StoreCartItem.vue';

const emit = defineEmits(['removeCartItemById', 'closeCart'])
const {cartItems} = defineProps({
  cartItems: {
    type: Object,
    required: true,
  },

})
const dialog = ref(null)
nextTick(() => dialog.value.showModal());

const totalSum = computed(() => {
  return Object.values(cartItems).reduce((acc, {
    quantity,
    productCard
  }) => acc += quantity * productCard.price, 0);
});

const isCartEmpty = computed(() => Object.keys(cartItems).length);

const closeCart = () => {
  emit('closeCart');
}

const removeCartItemById = (id) => {
  emit('removeCartItemById', id);
}

</script>

<style scoped lang="scss">
.cart {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  margin: 0;
  padding: 3rem;
  max-height: 60%;
  border: none;
  border-radius: 10px;

  &__content-wrapper {
    display: grid;
    row-gap: 2rem;
  }

  &__message-empty-cart,
  &__total-sum {
    font-weight: 600;
  }

  &__button-close-cart {
    position: absolute;
    width: 30px;
    height: 30px;
    top: 10px;
    right: 10px;
    border: none;

    &:before,
    &:after {
      background-color: #000;
      content: "";
      height: 100%;
      left: 50%;
      position: absolute;
      top: 50%;
      transform: translate(-50%, -50%) rotate(45deg);
      transition: .3s;
      width: 2px;
    }

    &:after {
      transform: translate(-50%, -50%) rotate(-45deg);
    }

    &:hover {
      &:before,
      &:after {
        background-color: red;
      }

      &:before {
        transform: translate(-50%, -50%) rotate(135deg);
      }

      &:after {
        transform: translate(-50%, -50%) rotate(45deg);
      }
    }
  }

  &::backdrop {
    background-color: rgba(0,0,0,.7);
  }
}

.cart-list {
  display: grid;
  row-gap: 1rem;
}
</style>