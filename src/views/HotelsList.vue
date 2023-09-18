<template>
  <div>
    <h4 class="hotels">Hotels</h4>
    <div class="hotel-cards-container">
      <div class="hotel-cards">
        <div class="hotel-card" v-for="hotel in hotels" :key="hotel.hotelId">
          <div class="hotel-card-inner">
            <h3>{{ hotel.name }}</h3>
            <p>Location: {{ hotel.location }}</p>
            <p>Star: {{ hotel.star }}</p>
            <router-link :to="'/hotels/' + hotel.hotelId">View More</router-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const hotels = ref([]);

onMounted(() => {
  // Fetch hotel data from JSON Server
  fetch("http://localhost:3000/hotels") // Replace with your JSON Server endpoint
    .then((response) => response.json())
    .then((data) => {
      hotels.value = data;
    });
});
</script>

<style scoped>
/* Your styles for HotelList.vue */
</style>
