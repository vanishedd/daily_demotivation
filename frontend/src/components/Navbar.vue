<template>
  <div class="navbar bg-base-100">
    <div class="flex-1">
      <a class="text-xl">daily demotivation</a>
    </div>
    <div class="flex-none">
      <button @click="showAlert" class="btn btn-square btn-ghost">
        get motivated instead
      </button>
    </div>
    <transition name="fade">
      <div v-if="isAlertVisible" role="alert" class="alert alert-error">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="stroke-current shrink-0 h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
          />
        </svg>
        <span>Error! Task failed successfully.</span>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isAlertVisible: false,
    };
  },
  methods: {
    showAlert() {
      this.isAlertVisible = true;

      // Hide the alert after 20 seconds
      setTimeout(() => {
        this.isAlertVisible = false;
      }, 20000);
    },
  },
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
