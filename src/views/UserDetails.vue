<template>
  <div class="user-details">
    <h4>User Details</h4>
    <hr />
    <div class="user-card">
      <div class="user-columns">
        <!-- Left Column (40% width) -->
        <div class="user-info">
          <div class="card">
            <img
              src="https://www.transparentpng.com/thumb/user/gray-user-profile-icon-png-fP8Q1P.png"
              class="card-img-top dp"
              alt="User Avatar"
            />
            <hr />
            <div class="card-body">
              <p>Name: {{ user.name }}</p>
              <p>Email: {{ user.email }}</p>
            </div>
          </div>
        </div>

        <!-- Right Column (60% width) -->
        <div class="ratings">
          <h6>Ratings given by user:</h6>
          <div class="rating-item" v-for="rating in userRatings" :key="rating.id">
            <div class="rating-content">
              <!-- Display rating information here -->
              <p>Hotel: {{ getHotelName(rating.hotelId) }}</p>
              <p>Rating: {{ rating.rating }}</p>
              <p>Comment: {{ rating.comment }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const userId = route.params.userId;
const user = ref({});
const userRatings = ref([]);
const hotels = ref([]);

onMounted(() => {
  // Fetch user data
  fetch(`http://localhost:3000/users/${userId}`)
    .then((response) => response.json())
    .then((data) => {
      user.value = data;
    })
    .catch((error) => {
      console.error("Error fetching user data:", error);
    });

  // Fetch hotels data
  fetch("http://localhost:3000/hotels")
    .then((response) => response.json())
    .then((data) => {
      hotels.value = data;
    });

  // Fetch user ratings data
  fetch(`http://localhost:3000/user-ratings/${userId}`)
    .then((response) => response.json())
    .then((data) => {
      userRatings.value = data;
    })
    .catch((error) => {
      console.error("Error fetching user ratings:", error);
    });
});

function getHotelName(hotelId) {
  const hotel = hotels.value.find((h) => h.hotelId === hotelId);
  return hotel ? hotel.name : "";
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
  max-width: 800px; /* Adjust the maximum width as needed */
  margin: 0 auto;
  text-align: left;
}
/*  */
.user-columns {
  display: flex; /* Use flexbox for column layout */
}

.user-info {
  flex: 40%; /* Left column takes up 40% width */
  margin-right: 20px; /* Add some spacing between columns */
}

.ratings {
  flex: 60%; /* Right column takes up 60% width */
  border: 1px solid #9e9b9b;
  border-radius: 5px;
  padding: 20px;
}

.rating-item {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-bottom: 10px;
}

.rating-content {
  text-align: left;
  border: 1px solid #ddd; /* Add a border to the rating-content div */
  padding: 10px; /* Adjust padding as needed */
}

.dp {
  height: 200px;
  width: 200px;
  margin: auto;
}
</style>

