<template>
    <div class="user-details">
      <h4>User Details</h4><hr>
      <div class="user-card">
        <div class="user-info">
          <p>Name: {{ user.name }}</p>
          <p>Email: {{ user.email }}</p>
        </div><hr>
        <div class="ratings">
          <h6>Ratings Given:</h6>
          <ul>
            <li v-for="rating in user.ratings" :key="rating.ratingId" class="rating-item">
              <div class="rating-info">
                <strong>Hotel name: {{ getHotelName(rating.hotelId) }}</strong>
                <p>Feedback: {{ rating.feedback }}</p>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref, onMounted } from 'vue';
  import { useRoute } from 'vue-router';
  
  const route = useRoute();
  const userId = route.params.userId;
  const user = ref({});
  const hotels = ref([]);
  
  onMounted(() => {
    fetch(`http://localhost:3000/users/${userId}`)
      .then((response) => response.json())
      .then((data) => {
        user.value = data;
      })
      .catch((error) => {
        console.error('Error fetching user data:', error);
      });
  
    fetch("http://localhost:3000/hotels")
      .then((response) => response.json())
      .then((data) => {
        hotels.value = data;
      });
  });
  
  function getHotelName(hotelId) {
    const hotel = hotels.value.find((h) => h.hotelId === hotelId);
    return hotel ? hotel.name : '';
  }
  </script>
  
  <style scoped>
  .user-details {
    text-align: center;
    padding: 20px;
  }
  
  .user-card {
    background-color: #f5f5f5;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px;
    max-width: 400px;
    margin: 0 auto;
    text-align: left;
  }
  
  .user-info {
    margin-bottom: 20px;
  }
  
  .ratings {
    text-align: left;
  }
  
  .rating-item {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 10px;
  }
  
  .rating-info {
    text-align: left;
  }
  </style>
  