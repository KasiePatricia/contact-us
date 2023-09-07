<script setup></script>

<template>
  <img src="../assets/images/contact-img.png" class="frame" alt="Frame Image" />
</template>

<style scoped>
.frame {
  display: none;
}
@media (min-width: 769px) {
  .frame {
    display: block;
    flex-basis: 50%;
    width: 50%;
    object-fit: cover;
  }
}
@media (min-width: 1025px) {
  .frame {
    flex-basis: 55%;
    width: 55%;
  }
}
</style>
