<template>
  <main id="app">
    <div class="container mb-3">
      <h1 class="text-center mt-2">Richard's Lessons</h1>
      <div class="d-flex">
        <button class="btn btn-primary fs-3 p-3 rounded-circle ms-auto" @click="showCheckOut" :disabled="!cartSize && showProduct">
          {{ cartSize }} <i class="bi bi-bag-plus"></i>
        </button>
      </div>
      <Lesson v-if="showProduct" :lessons="lessons" @add-item-to-cart="addToCart" />
      <Checkout v-else :cart="cart" @remove-item-from-cart="removeProduct" @checkout="checkout" />
    </div>
  </main>
</template>

<script>
import Lesson from './components/Lessons.vue';
import Checkout from './components/Checkout.vue';

export default {
  components: {
    Lesson,
    Checkout
  },
  data() {
    return {
      showProduct: true,
      lessons: [],
      searchTerm: "",
      sortAttribute: "subject",
      sortOrder: "asc",
      cart: [],
      username: "",
      phonenumber: "",
      backendURL: "https://cst-3145-coursework-2-backend.vercel.app/",
    };
  },
  created() {
    this.getLessons();
  },
  watch: {
    searchTerm: "getLessons",
    sortAttribute: "getLessons",
    sortOrder: "getLessons",
  },
  methods: {
    async getLessons() {
      const url = `${this.backendURL}lessons/?search=${this.searchTerm}&sort=${this.sortAttribute}&order=${this.sortOrder}`;
      const response = await fetch(url);
      this.lessons = await response.json();
    },
    addToCart(lesson) {
      if (lesson.spaces > 0) {
        lesson.spaces--;
        const cartIndex = this.cart.findIndex(i => i.lesson === lesson);
        if (cartIndex > -1) {
          this.cart[cartIndex].amount++;
        } else {
          this.cart.push({ lesson, amount: 1 });
        }
      }
    },
    removeProduct(lesson) {
      const index = this.cart.findIndex(i => i.lesson === lesson);
      if (index !== -1) {
        this.cart[index].amount--;
        lesson.spaces++;
        if (this.cart[index].amount == 0) {
          this.cart.splice(index, 1);
        }
      }
    },
    async checkout() {
      const order = {
        lessons: [...this.cart],
        username: this.username,
        phonenumber: this.phonenumber,
      };
      await fetch(`${this.backendURL}orders`, {
        method: "POST",
        headers: { "content-Type": "application/json" },
        body: JSON.stringify(order),
      });
      alert("Order successful!");
      this.showProduct = true;
      this.cart = []; // empty cart
    },
    showCheckOut() {
      this.showProduct = !this.showProduct;
    },
  },
  computed: {
    cartSize() {
      return this.cart.reduce((sum, lesson) => sum + lesson.amount, 0);
    }
  },
};
</script>

<style scoped>
/* Add your custom styles here */
</style>
