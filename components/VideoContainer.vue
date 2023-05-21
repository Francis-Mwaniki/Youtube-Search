<template>
  <transition name="popup" mode="out-in">
    <div v-if="videoId" class="fixed bottom-0 left-0 right-0 bg-gray-900 z-50">
      <div class="container mx-auto px-4 py-2">
        <div class="aspect-w-16 aspect-h-9">
          <iframe
            :src="`https://www.youtube.com/embed/${videoId}`"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
            class="w-full h-full"
          ></iframe>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    videoId: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
.popup-enter-active,
.popup-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}

.popup-enter,
.popup-leave-to {
  opacity: 0;
  transform: translateY(100%);
}
</style>
