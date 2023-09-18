<template>
  <div class="user-details">
    <h4>Hotel Details</h4>
    <hr />
    <div class="hotel-card" v-if="hotel.value">
      <div class="user-columns">
        <!-- Left Column (40% width) -->
        <div class="user-info">
          <div class="card">
            <img
              src="https://png.pngtree.com/png-clipart/20190705/original/pngtree-hotel-icon-for-personal-and-commercial-use-png-image_4341927.jpg"
              class="card-img-top dp"
              alt="User Avatar"
            />
            <hr />
            <div class="card-body">
              <h5>Hotel Name: {{ hotel.value.name }}</h5>
              <p>Location: {{ hotel.value.location }}</p>
              <p>Star: {{ hotel.value.star }}</p>
            </div>
          </div>
        </div>

        <!-- Right Column (60% width) -->
        <div class="ratings">
          <h6>Ratings given by users</h6>
          <div class="rating-item" v-for="rating in hotel.value.ratings" :key="rating.ratingId">
            <div class="rating-content">
              <h6>Username: {{ rating.user.name }}</h6>
              <p>Rating: {{ rating.rating }}</p>
              <p>Feedback: {{ rating.feedback }}</p>
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
const hotelId = route.params.hotelId;
const hotel = ref({
  value: {
    name: "Hotel Oberoi",
    location: "Marine Lines",
    star: "5",
    ratings: [
      {
        ratingId: "1",
        user: { name: "Manpreet" },
        rating: "3",
        feedback: "Great experience",
      },
      {
        ratingId: "2",
        user: { name: "Jayesh" },
        rating: "4",
        feedback: "Excellent staff, food, and service. Recommended",
      },
    ],
  },
});
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


