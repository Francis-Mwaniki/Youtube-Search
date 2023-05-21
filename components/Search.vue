<template>
  <div class="mt-4">
    <input
      type="text"
      v-model="searchTerm"
      @input="search"
      class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-blue-500 focus:border-blue-500"
      placeholder="Search"
    />

    <div v-if="searchResults.length" class="mt-2">
      <div class="border border-gray-300 rounded-md">
        <ul>
          <li
            v-for="result in searchResults"
            :key="result.videoId"
            class="flex items-center py-2 border-b border-gray-300"
          >
            <iframe
              width="200"
              height="150"
              :src="`https://www.youtube.com/embed/${result.videoId}`"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
              class="mr-2"
            ></iframe>

            <div>
              <h3 class="text-lg font-semibold">{{ result.videoTitle }}</h3>
              <p class="text-sm text-gray-600">{{ result.channelTitle }}</p>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const searchTerm = ref("");
    const searchResults = ref([]);
    const auth = useSupabaseAuthClient();
    const user = useSupabaseUser();

    const client = useSupabaseClient();

    async function search() {
      if (searchTerm.value.length < 1) {
        searchResults.value = [];
        return;
      }
      if (searchTerm.value.length == "") {
        searchResults.value = [];
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
      search,
    };
  },
};
</script>

<style>
/* Add any additional styles or customizations here */
</style>
