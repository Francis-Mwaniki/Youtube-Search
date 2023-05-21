<template>
  <div>
    <!-- Header -->
    <div class="bg-black py-4">
      <div class="container mx-auto flex justify-between items-center flex-row mt-12">
        <Loading v-if="loading" />
        <h1 class="text-white text-2xl font-bold">Machine Learning</h1>
        <!-- user profile -->
        <div class="flex items-center">
          <img
            src="https://images.pexels.com/photos/1181298/pexels-photo-1181298.jpeg?auto=compress&cs=tinysrgb&w=600"
            alt="User Profile"
            class="w-10 h-10 rounded-full mr-2"
          />
          <h2 class="text-white text-lg font-bold">
            {{ user.email ? user.email : "Not Authenticated" }}
          </h2>
          <!-- button for signout -->
          <button
            @click="signout"
            class="bg-gray-700 text-white py-2 px-4 rounded hover:bg-gray-600 ml-4"
          >
            Sign Out
          </button>
        </div>
      </div>
    </div>

    <Youtube />
    <!-- Hero -->
    <div class="bg-gray-100 py-16">
      <div
        class="container mx-auto flex flex-col lg:flex-row items-center justify-center lg:justify-between"
      >
        <div class="lg:w-1/2 flex-col flex gap-y-3 justify-center items-center mx-auto">
          <h2 class="text-3xl font-bold mb-4">What is Machine Learning?</h2>
          <p class="text-gray-600">
            Machine learning is a method of teaching computers to learn from data, without
            being explicitly programmed. It is a subset of artificial intelligence that
            focuses on building algorithms that can make predictions or decisions based on
            input data.
          </p>
          <a
            href="#"
            class="bg-indigo-600 text-white py-2 px-4 rounded hover:bg-indigo-700 mt-4 flex justify-center justify-self-center items-center mx-auto"
            >Learn More</a
          >
        </div>
        <div class="lg:w-1/2">
          <img
            src="https://images.pexels.com/photos/1181298/pexels-photo-1181298.jpeg?auto=compress&cs=tinysrgb&w=600"
            alt="Machine Learning"
            class="rounded-lg shadow-lg"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const auth = useSupabaseAuthClient();
const user = useSupabaseUser();
const client = useSupabaseClient();
const router = useRouter();
const success = ref("");
const errMsg = ref("");
const loading = ref(false);

definePageMeta({
  middleware: "auth",
});

/* sigout user */
const signout = async () => {
  loading.value = true;
  const { error } = await auth.auth.signOut();
  setTimeout(() => {
    loading.value = false;
  }, 3000);
  if (error) {
    loading.value = false;
    errMsg.value = error.message;
  }
  success.value = "Signed out successfully";
  setTimeout(() => {
    success.value = "";
    router.push("Login");
  }, 3000);
};
</script>
