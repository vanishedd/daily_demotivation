<template>
  <div
    class="prose contents flex flex-col justify-center items-center h-screen"
  >
    <div class="text-center pt-20">
      <p>{{ currentDate }}</p>
      <h1 class="text-white w-1/2 inline-block">
        {{ cachedQuote || todayQuote }}
      </h1>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import { supabase } from "../lib/supabaseClient";

export default {
  data() {
    return {
      todayQuote: null,
      cachedQuote: this.getCachedQuote(),
      currentDate: this.getCurrentDate(),
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    getCurrentDate() {
      const options = {
        weekday: "long",
        day: "numeric",
        month: "long",
        year: "numeric",
      };
      return new Date().toLocaleDateString("en-US", options);
    },
    getCachedQuote() {
      // Retrieve the cached quote from localStorage
      return localStorage.getItem("cachedQuote");
    },
    setCachedQuote(quote) {
      // Cache the quote in localStorage
      localStorage.setItem("cachedQuote", quote);
    },
    async fetchData() {
      const todayDate = new Date().toISOString().split("T")[0];
      console.log(todayDate);

      // Check if there's a cached quote
      if (this.cachedQuote) {
        this.todayQuote = this.cachedQuote;
      }

      const { data, error } = await supabase
        .from("quotes")
        .select("quote")
        .eq("date", todayDate);

      if (error) {
        console.error("Error fetching data:", error.message);
      } else {
        if (data.length > 0) {
          // Assuming there's only one row with today's date, you can access the quote
          this.todayQuote = data[0].quote;
          // Cache the quote for future use
          this.setCachedQuote(this.todayQuote);
        } else {
          this.todayQuote = "No quote found for today";
        }
      }
    },
  },
};
</script>
