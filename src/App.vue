<template>
  <div
    class="flex flex-col min-h-screen"
    :class="{ dark: isDarkMode, 'bg-gray-100': !isDarkMode }"
  >
    <!-- Navbar -->
    <div class="navbar" :class="navbarClass">
      <div class="flex-1">
        <a class="btn btn-ghost normal-case text-xl" :class="textClass"
          >Daily Demotivation</a
        >
      </div>
      <div class="flex-none">
        <button
          class="btn btn-ghost"
          :class="textClass"
          @click="openMonthlyReviewModal"
        >
          Monthly Review
        </button>
        <button class="btn btn-square btn-ghost" @click="toggleDarkMode">
          <svg
            v-if="isDarkMode"
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z"
            />
          </svg>
          <svg
            v-else
            xmlns="http://www.w3.org/2000/svg"
            class="h-6 w-6"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
            />
          </svg>
        </button>
      </div>
    </div>

    <!-- Main Content -->
    <div class="flex-grow flex items-center justify-center px-4 py-8">
      <div class="text-center w-full max-w-lg">
        <p
          v-if="!currentQuote.date"
          class="mb-4 text-xl font-semibold"
          :class="textClass"
        >
          <span class="skeleton-box w-48 h-8"></span>
        </p>
        <p v-else class="mb-4 text-xl font-semibold" :class="textClass">
          {{ formattedDate }}
        </p>

        <h1
          v-if="!currentQuote.quote"
          class="text-2xl sm:text-4xl font-bold mb-8"
          :class="textClass"
        >
          <span class="skeleton-box w-full h-12 sm:h-16"></span>
        </h1>
        <h1
          v-else
          class="text-2xl sm:text-4xl font-bold mb-8"
          :class="textClass"
        >
          {{ currentQuote.quote }}
        </h1>

        <div class="flex flex-col sm:flex-row justify-center items-center mb-8">
          <button
            class="btn btn-lg mb-4 sm:mb-0 sm:mr-4 transition-colors duration-300"
            :class="[
              votedType === 'upvotes'
                ? isDarkMode
                  ? 'btn-success'
                  : 'bg-green-500 text-white'
                : isDarkMode
                ? 'btn-outline-success hover:bg-success hover:text-white'
                : 'bg-white text-green-500 hover:bg-green-500 hover:text-white border border-green-500',
              textClass,
            ]"
            @click="vote('upvotes')"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6 mr-2"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M5 15l7-7 7 7"
              />
            </svg>
            {{ currentQuote.upvotes || 0 }}
          </button>
          <button
            class="btn btn-lg transition-colors duration-300"
            :class="[
              votedType === 'downvotes'
                ? isDarkMode
                  ? 'btn-error'
                  : 'bg-red-500 text-white'
                : isDarkMode
                ? 'btn-outline-error hover:bg-error hover:text-white'
                : 'bg-white text-red-500 hover:bg-red-500 hover:text-white border border-red-500',
              textClass,
            ]"
            @click="vote('downvotes')"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6 mr-2"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M19 9l-7 7-7-7"
              />
            </svg>
            {{ currentQuote.downvotes || 0 }}
          </button>
        </div>
      </div>
    </div>

    <!-- Monthly Review Modal -->
    <div
      v-if="isMonthlyReviewModalOpen"
      class="modal"
      :class="{ 'modal-open': isMonthlyReviewModalOpen }"
    >
      <div class="modal-box" :class="{ 'bg-white': !isDarkMode }">
        <h3 class="font-bold text-lg" :class="textClass">Monthly Review</h3>
        <p v-if="topMonthlyQuotes.length > 0" class="py-4" :class="textClass">
          Here are the top quotes for this month:
        </p>
        <ul v-if="topMonthlyQuotes.length > 0" class="mb-4 space-y-4">
          <li
            v-for="(quote, index) in topMonthlyQuotes"
            :key="quote.id"
            class="border rounded-lg p-4"
            :class="{ 'bg-gray-800': isDarkMode, 'bg-gray-100': !isDarkMode }"
          >
            <div class="font-bold mb-2" :class="textClass">
              Quote #{{ index + 1 }}
            </div>
            <div class="mb-2" :class="textClass">{{ quote.quote }}</div>
            <div class="text-sm" :class="textClass">
              {{ quote.upvotes }} upvotes
            </div>
          </li>
        </ul>
        <p v-else class="py-4" :class="textClass">
          No quotes available for this month.
        </p>
        <div class="modal-action">
          <button
            class="btn"
            :class="{ 'btn-primary': isDarkMode, 'btn-outline': !isDarkMode }"
            @click="closeMonthlyReviewModal"
          >
            Close
          </button>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="footer footer-center p-4" :class="footerClass">
      <div>
        <p :class="textClass">Made with &lt;3 by vanishedd</p>
      </div>
    </footer>
  </div>
</template>

<script>
import { supabase } from "./lib/supabaseClient";

export default {
  data() {
    return {
      currentQuote: {},
      topMonthlyQuotes: [],
      isDarkMode: true,
      votedType: null,
      isMonthlyReviewModalOpen: false,
    };
  },
  computed: {
    navbarClass() {
      return this.isDarkMode
        ? "bg-gray-800"
        : "bg-white border-b border-gray-200";
    },
    footerClass() {
      return this.isDarkMode
        ? "bg-gray-800 text-white"
        : "bg-white text-gray-800 border-t border-gray-200";
    },
    textClass() {
      return this.isDarkMode ? "text-white" : "text-gray-800";
    },
    formattedDate() {
      const options = {
        weekday: "long",
        day: "2-digit",
        month: "2-digit",
        year: "numeric",
      };
      return new Date(this.currentQuote.date).toLocaleDateString(
        undefined,
        options
      );
    },
  },
  methods: {
    async fetchQuote() {
      const today = new Date().toISOString().slice(0, 10);

      if (this.currentDate !== today) {
        this.currentDate = today;
        const { data, error } = await supabase
          .from("quotes")
          .select("*")
          .eq("date", today)
          .single();

        if (error) {
          console.error("Error fetching quote:", error);
        } else {
          this.currentQuote = data;
          if (!data.shown) {
            await this.markQuoteAsShown(data.id);
          }
        }
      }
    },
    async markQuoteAsShown(quoteId) {
      const { data, error } = await supabase
        .from("quotes")
        .update({ shown: true })
        .eq("id", quoteId);

      if (error) {
        console.error("Error marking quote as shown:", error);
      }
    },
    async fetchTopMonthlyQuotes() {
      const currentDate = new Date();
      const startOfMonth = new Date(
        currentDate.getFullYear(),
        currentDate.getMonth(),
        1
      )
        .toISOString()
        .slice(0, 10);
      const endOfMonth = new Date(
        currentDate.getFullYear(),
        currentDate.getMonth() + 1,
        0
      )
        .toISOString()
        .slice(0, 10);

      const { data, error } = await supabase
        .from("quotes")
        .select("*")
        .gte("date", startOfMonth)
        .lte("date", endOfMonth)
        .eq("shown", true)
        .order("upvotes", { ascending: false })
        .limit(3);

      if (error) {
        console.error("Error fetching top monthly quotes:", error);
      } else {
        this.topMonthlyQuotes = data;
      }
    },
    async vote(type) {
      if (this.votedType !== type) {
        const previousVoteType = this.votedType;
        const { data, error } = await supabase
          .from("quotes")
          .update({
            [type]: this.currentQuote[type] + 1,
            [previousVoteType]: previousVoteType
              ? this.currentQuote[previousVoteType] - 1
              : this.currentQuote[previousVoteType],
          })
          .eq("id", this.currentQuote.id);
        if (error) {
          console.error("Error updating votes:", error);
        } else {
          this.votedType = type;
          localStorage.setItem("votedType", type);
          await this.fetchQuote();
        }
      }
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
    },
    openMonthlyReviewModal() {
      this.fetchTopMonthlyQuotes();
      this.isMonthlyReviewModalOpen = true;
    },
    closeMonthlyReviewModal() {
      this.isMonthlyReviewModalOpen = false;
    },
  },
  async mounted() {
    await this.fetchQuote();
    this.votedType = localStorage.getItem("votedType");
  },
  async created() {
    await this.fetchQuote();
    this.votedType = localStorage.getItem("votedType");
  },
};
</script>
