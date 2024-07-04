<template>
    <div class="container">
      <div class="d-flex align-items-end gap-3 flex-wrap">
        <div>
          <input type="text" class="form-control" v-model="searchTerm" placeholder="Search for a lesson..." />
        </div>
        <div>
          <label for="sortAttribute">Sort By:</label>
          <select v-model="sortAttribute" class="form-select">
            <option value="title">Title</option>
            <option value="subject">Subject</option>
            <option value="location">Location</option>
            <option value="price">Price</option>
            <option value="spaces">Spaces</option>
          </select>
        </div>
        <div>
          <label for="sortOrder">Sort Order:</label>
          <select v-model="sortOrder" class="form-select">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
          </select>
        </div>
      </div>
      <div class="row">
        <div class="col-sm-4" v-for="product in lessons" :key="product.id">
          <div class="card mb-3 rounded-0 border border-success-subtle">
            <div class="card-header d-flex">
              <div>
                <p>Subject: {{ product.subject }}</p>
                <h5>Title: {{ product.title }}</h5>
              </div>
              <i :class="product.icon" class="fs-1 ms-auto"></i>
            </div>
            <div class="card-body">
              <p>Price: {{ product.price }} AED</p>
              <p>Spaces: {{ product.spaces }}</p>
              <p>Location: {{ product.location }}</p>
            </div>
            <div class="card-footer">
              <button class="btn btn-primary rounded-0" @click="$emit('add-item-to-cart', product)" :disabled="product.spaces === 0">
                Add to cart
              </button>
              <button class="btn btn-danger rounded-0" @click="$emit('remove-item-from-cart', product)">
                X
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Lessons',
    props: ['lessons'],
    data() {
      return {
        searchTerm: "",
        sortAttribute: "subject",
        sortOrder: "asc",
      };
    },
  };
  </script>
  
  <style scoped>
  /* Add your custom styles here */
  </style>
  