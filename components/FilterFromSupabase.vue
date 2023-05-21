<template>
  <!-- Tailwindcss component for youtube searches -->
  <main class="py-6 px-6 mx-auto">
    <Search />
    <Loading v-if="loading" />
    <h1 class="text-2xl font-semibold">YouTube Search</h1>

    <div class="mt-4">
      <div class="flex space-x-4">
        <button
          @click="searchCourses('python')"
          class="py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          Python
        </button>
        <button
          @click="searchCourses('datascience')"
          class="py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          Data Science
        </button>
        <button
          @click="searchCourses('artificialintelligence')"
          class="py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          Artificial Intelligence
        </button>
        <button
          @click="searchCourses('javascript')"
          class="py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          JavaScript
        </button>
        <button
          @click="searchCourses('typescript')"
          class="py-2 px-4 bg-blue-500 text-white rounded-md"
        >
          TypeScript
        </button>
      </div>

      <div v-if="searchResults.length" class="mt-4">
        <div class="border border-gray-500 rounded-md">
          <ul>
            <li
              v-for="result in searchResults"
              :key="result.videoId"
              class="flex items-center py-2 border-b border-gray-300"
            >
              <!-- frame for youtube with video id -->
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

      <div v-else class="mt-2">
        <p class="text-gray-600">No results found.</p>
      </div>
    </div>
  </main>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const searchTerm = ref("");
    const loading = ref(false);
    const searchResults = ref([]);

    const client = useSupabaseClient();

    async function searchCourses(query) {
      searchTerm.value = query;

      try {
        const { data, error } = await client
          .from("youtube")
          .select("*")
          .ilike("video_name", `%${searchTerm.value}%`)
          .limit(10);

        if (error) {
          console.error("Error fetching search results:", error);
        } else {
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
      loading,
      searchResults,
      searchCourses,
    };
  },
};
</script>

<style>
/* Add Tailwind CSS classes or custom styles here */
</style>
