<template>
  <div>
    <NuxtPage />
  </div>
  <!--create a footer monitored and developed by francis with love -->
  <footer class="bg-black py-4 shadow fixed inset-x-0 bottom-0 mb-0">
    <div class="flex justify-center items-center mx-auto gap-x-2">
      <!--  home  svg-->

      <button
        @click="goBackHome"
        class="cursor-pointer flex flex-row justify-center items-center bg-slate-900 ring-2 ring-slate-900 rounded-md px-4 py-2"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6 text-white"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M10 19l-7-7m0 0l7-7m-7 7h18"
          />
        </svg>
        <p class="text-white"><span>Go back</span></p>
      </button>

      <p class="text-white">
        Monitored & Developed by Francis with love In collaboaration with Everlyne
      </p>
    </div>
  </footer>
</template>
<script>
export default {
  methods: {
    goBackHome() {
      this.$router.push("/");
    },
  },
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=EB+Garamond:ital@1&family=Poppins&family=Quicksand:wght@600&display=swap");
* {
  font-family: "EB Garamond", serif;
  font-family: "Poppins", sans-serif;
  font-family: "Quicksand", sans-serif;
}
</style>
