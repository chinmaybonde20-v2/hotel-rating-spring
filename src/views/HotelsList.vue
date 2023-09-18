<template>
  <div>
    <h4 class="hotels">Hotels List</h4>
    <div class="hotel-cards-container">
      <div class="hotel-cards">
        <div class="hotel-card" v-for="hotel in hotels" :key="hotel.hotelId">
          <div class="hotel-card-inner">
            <h3>{{ hotel.name }}</h3>
            <p>Location: {{ hotel.location }}</p>
            <p>Star: {{ hotel.star }}</p>
          </div>
          <button class="view-details-button" @click="navigateToHotelDetails(hotel.hotelId)">View Hotel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const hotels = ref([]);

onMounted(() => {
  // Fetch hotel data from JSON Server
  fetch("http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/hotels")
    .then((response) => response.json())
    .then((data) => {
      hotels.value = data;
    });
});
function navigateToHotelDetails(hotelId) {
  router.push(`/hotels/${hotelId}`);
}
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

.view-details-button {
  background-color: #007bff; /* Blue color for the button */
  color: #fff; /* White text color */
  padding: 5px; /* Adjust padding for the button */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease; /* Add hover effect */
  margin: auto;
}

.view-details-button:hover {
  background-color: #0056b3; /* Darker blue color on hover */
}
</style>
