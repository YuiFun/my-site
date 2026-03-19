<script setup lang="ts">
import { onMounted, ref } from 'vue';

  const ipAddress=ref("Loading...")
  const getIP=async function getIP() {
    const response = await fetch('https://api.ipify.org?format=json');
    const data = await response.json();
    
    ipAddress.value = data.ip;
  }
onMounted(() => {
  getIP();
});

</script>

<template>
  <p id="userIP">{{ "Welcome"+ipAddress }}</p>
</template>

<style scoped></style>
