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
          <div>
            <h6>Leave rating for hotel</h6>
            <div>
              <button @click="showRatingForm">Add rating</button>
              <div v-if="showForm">
                <div>
                  <label for="hotelDropdown">Select Hotel:</label>
                  <select v-model="selectedHotel" id="hotelDropdown">
                    <option v-for="hotel in hotels" :value="hotel.hotelId" :key="hotel.hotelId">{{ hotel.name }}</option>
                  </select>
                </div>
                <div>
                  <label for="ratingDropdown">Rating:</label>
                  <select v-model="selectedRating" id="ratingDropdown">
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                  </select>
                </div>
                <div>
                  <label for="feedback">Feedback:</label>
                  <input v-model="feedback" type="text" id="feedback" />
                </div>
                <button @click="submitRating">Submit</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Column (60% width) -->
        <div class="ratings">
          <h6>Ratings given by user</h6>
          <div
            class="rating-item"
            v-for="rating in userRatings"
            :key="rating.ratingId"
          >
            <div class="rating-content">
              <p>Hotel: {{ getHotelName(rating.hotelId) }}</p>
              <p>Location: {{ getHotelLocation(rating.hotelId) }}</p>
              <p>Star: {{ getHotelStar(rating.hotelId) }}</p>
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
const userId = route.params.userId;
const user = ref({});
const userRatings = ref([]);
const hotels = ref([]);
const showForm = ref(false);
const selectedHotel = ref(null);
const selectedRating = ref(null);
const feedback = ref("");
let nextRatingId = 10;

onMounted(() => {
  // Fetch user data
  fetch(
    `http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/users/${userId}`
  )
    .then((response) => response.json())
    .then((data) => {
      user.value = data;
    })
    .catch((error) => {
      console.error("Error fetching user data:", error);
    });

  // Fetch hotels data
  fetch(
    "http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/hotels"
  )
    .then((response) => response.json())
    .then((data) => {
      hotels.value = data;
    });

  // Fetch user ratings data
  fetch(
    `http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/ratings/users/${userId}`
  )
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

function getHotelLocation(hotelId) {
  const hotel = hotels.value.find((h) => h.hotelId === hotelId);
  return hotel ? hotel.location : "";
}

function getHotelStar(hotelId) {
  const hotel = hotels.value.find((h) => h.hotelId === hotelId);
  return hotel ? hotel.star : "";
}

const showRatingForm = () => {
  showForm.value = true;
};

const submitRating = () => {
  if (!selectedHotel.value || !selectedRating.value || !feedback.value) {
    alert("Please fill in all fields.");
    return;
  }

  const newRating = {
    ratingId: nextRatingId++,
    userId: userId,
    hotelId: selectedHotel.value,
    rating: selectedRating.value,
    feedback: feedback.value,
  };

  fetch("http://microservice-alb-1225467990.ap-south-1.elb.amazonaws.com/ratings/addrating", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(newRating),
  })
    .then((response) => response.json())
    .then((data) => {
      console.log("Rating added successfully:", data);
      userRatings.value.push(data);
      selectedHotel.value = null;
      selectedRating.value = null;
      feedback.value = "";
      showForm.value = false;
    })
    .catch((error) => {
      console.error("Error adding rating:", error);
    });
};
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
  max-width: 800px;
  margin: 0 auto;
  text-align: left;
}

.user-columns {
  display: flex;
}

.user-info {
  flex: 40%;
  margin-right: 20px;
}

.ratings {
  flex: 60%;
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
  border: 1px solid #ddd;
  padding: 10px;
}

.dp {
  height: 200px;
  width: 200px;
  margin: auto;
}
</style>
