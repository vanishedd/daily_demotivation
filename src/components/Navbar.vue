<template>
  <div class="navbar bg-base-100">
    <div class="flex-1">
      <a class="text-xl font-bold">daily demotivation</a>
    </div>
    <div class="gap-x-1">
      <button @click="openModal" class="btn btn-ghost">
        get motivated instead
      </button>
      <div class="dropdown dropdown-end">
        <div tabindex="0" role="button" class="btn btn-ghost rounded-btn">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            class="inline-block w-5 h-5 stroke-current"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M5 12h.01M12 12h.01M19 12h.01M6 12a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0zm7 0a1 1 0 11-2 0 1 1 0 012 0z"
            ></path>
          </svg>
        </div>
        <ul
          tabindex="0"
          class="menu dropdown-content z-[1] p-2 shadow bg-base-100 bg-opacity-80 rounded-box w-52 mt-4"
        >
          <li><a>techstack</a></li>
          <li>
            <a href="https://github.com/vanishedd/daily_demotivation">repo</a>
          </li>
        </ul>
      </div>
    </div>
    <Alert
      :is-visible="isModalVisible"
      @close="closeModal"
      modalId="get_motivated"
    />
  </div>
</template>

<script>
  import Alert from "./Alert.vue";

  export default {
    data() {
      return {
        isModalVisible: false,
        alerts: [],
        alertCount: 0,
      };
    },
    methods: {
      openModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      },
    },
    components: {
      Alert,
    },
  };
</script>
