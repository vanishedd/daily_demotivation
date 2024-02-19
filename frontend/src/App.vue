<script setup>
import navbar from "./components/Navbar.vue";
import daily from "./components/DailyQuote.vue";
import footerVue from "./components/Footer.vue";
import { Analytics } from "@vercel/analytics";
</script>

<template>
  <div class="flex flex-col h-screen">
    <Analytics />
    <navbar />
    <daily />
    <footerVue />
  </div>
</template>
