<template>
  <div
    class="mt-4"
    :class="
      isSearching
        ? 'inset-0 fixed z-10 opacity-100 bg-black overflow-auto'
        : 'bg-black p-4 '
    "
  >
    <Loading v-if="loading" class="inset-0 z-50" />
    <div
      class="flex flex-row justify-between mx-auto items-center max-w-5xl"
      :class="isSearching ? 'max-w-5xl mx-auto' : ''"
    >
      <input
        type="search"
        v-model="searchTerm"
        @input="search"
        @focus="isSearching = true"
        class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-blue-500 focus:border-blue-500"
        :class="
          isSearching
            ? 'top-1 max-w-5xl mx-auto inset-x-0 text-white fixed z-10 opacity-90 bg-black '
            : ''
        "
        placeholder="Looking for a course? Let's find it for you!"
      />
      <!-- cancel icon -->
      <button
        v-if="isSearching"
        @click="
          isSearching = false;
          searchTerm = '';
          searchResults = [];
        "
        class="text-white fixed z-10 bg-black top-2 right-2"
      >
        <!-- icon for cancel -->
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="w-6 h-6 text-gray-600"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M6 18L18 6M6 6l12 12"
          />
        </svg>
        close
      </button>
    </div>
    <!-- display videos -->
    <div v-if="searchResults.length > 0" class="flex flex-col max-w-5xl mx-auto mt-4">
      <h2 class="text-lg font-bold mb-4 text-center mt-14 text-white">Recommendation</h2>

      <div class="grid grid-cols-5 gap-4 bg-white">
        <!-- Recommendation -->

        <div
          v-for="result in searchResults"
          :key="result.videoId"
          class="flex flex-col justify-between items-center rounded-md p-2"
        >
          <div class="flex flex-col bg-black text-white">
            <h3 class="text-lg font-bold">{{ result.video_name }}</h3>
            <p class="text-gray-600">{{ result.channel_name }}</p>
          </div>
          <div class="aspect-w-16 aspect-h-9 mb-2">
            <iframe
              :src="'https://www.youtube.com/embed/' + result.videoId"
              frameborder="0"
              allowfullscreen
              class="w-full h-full"
            ></iframe>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!--  <VideoContainer
    :videoId="videoId"
    v-if="showVideo"
    class="max-w-5xl fixed inset-x-0 z-30"
  /> -->
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const searchTerm = ref("");
    const searchResults = ref([]);
    const auth = useSupabaseAuthClient();
    const user = useSupabaseUser();
    const isSearching = ref(false);
    const showVideo = ref(false);
    const loading = ref(false);
    const videoId = ref("");

    const client = useSupabaseClient();

    const openVideo = (videoId) => {
      setTimeout(() => {
        loading.value = true;
        showVideo.value = true;
        videoId.value = videoId;
      }, 1000);
      setTimeout(() => {
        loading.value = false;
      }, 2000);
    };

    async function search() {
      if (searchTerm.value.length < 1) {
        searchResults.value = [];
        return;
      }

      isSearching.value = true;
      loading.value = true;

      try {
        const { data, error } = await client
          .from("youtube")
          .select("*")
          .ilike("video_name", `%${searchTerm.value}%`)
          .limit(10);

        if (error) {
          console.error("Error fetching search results:", error);
        } else {
          console.log(data);
          searchResults.value = data.map((item) => ({
            videoId: item.video_id,
            videoName: item.video_name,
            channelName: item.channel_name,
            thumbnailUrl: item.thumbnail_url,
          }));
        }
      } catch (error) {
        console.error("Error fetching search results:", error);
      }

      loading.value = false;
      setTimeout(() => {
        isSearching.value = false;
      }, 3000);
    }

    // Watch for changes in the search term and trigger the search function
    watch(searchTerm, search);

    return {
      searchTerm,
      searchResults,
      isSearching,
      search,
      openVideo,
      showVideo,
      videoId,
      loading,
    };
  },
};
</script>

<style>
/* Add any additional styles or customizations here */
</style>
