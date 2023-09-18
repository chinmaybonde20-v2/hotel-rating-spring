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
  fetch("http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/hotels")
    .then((response) => response.json())
    .then((data) => {
      hotels.value = data;
    });
});
</script>

<style scoped>
.hotel-cards-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.hotel-cards {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.hotel-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  width: 300px;
}

.hotel-card h3 {
  font-size: 20px;
  margin: 0;
}

.hotel-card p {
  margin: 10px 0;
}

.hotels {
  text-align: center;
  margin: 20px;
  margin-bottom: 40px;
}
</style>
