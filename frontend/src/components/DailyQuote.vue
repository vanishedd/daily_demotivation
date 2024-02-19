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
    <alert :is-visible="isAlertVisible" />
  </div>
</template>

<script>
import Alert from "./Alert.vue";

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
  components: {
    Alert,
  },
};
</script>
