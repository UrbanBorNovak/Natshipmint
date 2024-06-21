<template>
  <div class="ship">
    <div>{{ ship.name }}</div>
    <div>{{ ship.size }}</div>
    <div>{{ ship.length }}</div>
    <!-- Include SVG visualization logic here -->
  </div>
</template>

<script>
export default {
  name: 'Ship',
  props: {
    ship: Object
  }
};
</script>

<style scoped>
.ship {
  border: 1px solid #ccc;
  padding: 10px;
  width: 200px;
}
</style>
