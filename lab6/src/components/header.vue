<template>
  <div class="location-box">
    <div class="location">
      <i class="fas fa-map-marker-alt"></i> {{ location }}
    </div>
    <div class="date">
      <i class="far fa-calendar-alt"></i> {{ currentDate }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'GeolocatedHeader',
  props: {
    location: String,
    currentDate: String
  }
}
</script>

<style>
.location-box {
  text-align: center;
  color: #FFF;
  font-size: 1.5rem;
  margin-bottom: 20px;
}

.location-box i {
  margin-right: 5px;
}
</style>
