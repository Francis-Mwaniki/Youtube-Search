<template>
  <div
    class="mt-4"
    :class="
      isSearching
        ? 'inset-0 fixed z-10 opacity-100 bg-black overflow-auto'
        : 'bg-black p-4 '
    "
  >
    <Loading v-if="Loading" class="inset-0 z-50" />
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
        placeholder="Search"
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

    <div v-if="searchResults.length" class="mt-8 overflow-y-auto max-w-6xl mx-auto">
      <div class="border border-gray-300 rounded-md overflow-auto">
        <ul>
          <li
            v-for="result in searchResults"
            :key="result.videoId"
            class="flex items-center py-2 border-b border-gray-300 overflow-auto"
          >
            <transition
              name="popup"
              mode="out-in"
              class="fixed bottom-0 left-0 right-0 bg-gray-900 z-40 top-0 max-w-4xl mx-auto my-auto"
              v-if="showVideo"
            >
              <div v-if="showVideo">
                <div class="container mx-auto px-4 py-2">
                  <div
                    class="aspect-w-16 aspect-h-9 flex justify-center items-center flex-col"
                  >
                    <iframe
                      width="200"
                      height="150"
                      :src="`https://www.youtube.com/embed/${result.videoId}`"
                      frameborder="0"
                      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                      allowfullscreen
                      class="py-64"
                    ></iframe>
                    <!-- not showing alert -->
                    <div class="text-white">
                      <p>Not showing? Sorry</p>
                    </div>

                    <!-- youtube watch link -->

                    <a
                      :href="`https://www.youtube.com/watch?v=${result.videoId}`"
                      target="_blank"
                      class="text-white"
                    >
                      Watch on Youtube
                    </a>

                    <button
                      @click="showVideo = false"
                      class="text-white fixed z-10 bg-black bottom-4 inset-y-2 h-5 w-5"
                    >
                      <!-- icon for close -->
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
                  <!-- close -->
                </div>
              </div>
            </transition>
            <div>
              <h3 class="text-lg font-semibold text-white">{{ result.videoTitle }}</h3>
              <p class="text-sm text-gray-200">{{ result.channelTitle }}</p>
            </div>
            <!-- <Icon name youtube -->
            <button
              @click="openVideo(result.videoId)"
              class="flex items-center px-4 py-2 ml-auto text-gray-600"
            >
              <!-- icon for redirect -->
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="w-6 h-6 ml-auto text-gray-600"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M14 5l7 7m0 0l-7 7m7-7H3"
                />
              </svg>
            </button>
          </li>
        </ul>
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
        searchTerm.value = "";
        return;
      }
      if (searchTerm.value.length == "") {
        searchResults.value = [];
        searchTerm.value = "";
        return;
      }

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
            videoTitle: item.video_name,
            channelTitle: item.channel_name,
            thumbnailUrl: item.thumbnail_url,
          }));
        }
      } catch (error) {
        console.error("Error fetching search results:", error);
      }
    }

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
