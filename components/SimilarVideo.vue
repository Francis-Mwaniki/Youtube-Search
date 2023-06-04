<template>
  <div class="flex flex-col justify-between items-center rounded-md p-2">
    <div class="flex flex-col bg-black text-white">
      <h3 class="text-lg font-bold">{{ videoName }}</h3>
      <p class="text-gray-600">{{ channelName }}</p>
    </div>
    <div class="aspect-w-16 aspect-h-9 mb-2">
      <iframe
        :src="'https://www.youtube.com/embed/' + videoId"
        frameborder="0"
        allowfullscreen
        class="w-full h-full"
      ></iframe>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    videoId: {
      type: String,
      required: true,
    },
    videoName: {
      type: String,
      required: true,
    },
    channelName: {
      type: String,
      required: true,
    },
  },
};
</script>

<style>
/* Add any additional styles or customizations here */
</style>
