<template>
  <router-view/>
    <FooterSection />
</template>

<script setup>
import FooterSection from './components/FooterSection.vue';
</script>

<style>
body {
  padding: 0;
  margin: 0;
  background-image: url('assets/images/background.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  font-family: Arial, Helvetica, sans-serif;
  color: white;
}
</style>
