<script setup>
  import navbar from "./components/Navbar.vue";
  import daily from "./components/DailyQuote.vue";
  import footerVue from "./components/Footer.vue";
</script>

<template>
  <div class="flex flex-col h-screen">
    <navbar />
    <daily />
    <footerVue />
  </div>
</template>
