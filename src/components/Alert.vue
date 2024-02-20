<!-- Alert.vue -->
<template>
  <dialog
    :id="get_motivated"
    class="modal"
    :open="isVisible"
    @close="closeModal"
  >
    <div class="modal-overlay" @click="closeModal"></div>
    <div class="modal-box">
      <h3 class="font-bold text-lg">Dear User!</h3>
      <p class="py-4">
        This link is deactivated. Clicking this link may cause irreparable
        damage to you. For security reasons, we have decided not to make this
        option available to you.
      </p>
      <div class="modal-action">
        <form @submit.prevent="closeModal" method="dialog">
          <button class="btn">close</button>
        </form>
      </div>
    </div>
  </dialog>
</template>

<script>
  export default {
    props: {
      isVisible: Boolean,
      modalId: String,
    },
    methods: {
      closeModal() {
        this.$emit("close");
      },
    },
  };
</script>

<style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(
      0,
      0,
      0,
      0.5
    ); /* Adjust the alpha value for the desired transparency */
    z-index: 999;
  }

  .modal-box {
    position: relative;
    z-index: 1000;
    /* Add your other modal styles here */
  }
</style>
