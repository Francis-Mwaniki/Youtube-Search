<template>
  <div>
    <nav class="bg-black py-4 shadow fixed inset-x-0 top-0 mb-4">
      <ul class="flex justify-center items-center mx-auto gap-x-2">
        <li class="inline-block">
          <NuxtLink class="text-white px-4 py-2" to="/">Home</NuxtLink>
        </li>

        <li class="inline-block">
          <NuxtLink class="text-white px-4 py-2" to="Dashboard">Dashboard</NuxtLink>
        </li>
      </ul>
    </nav>
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
    <div class="lg:w-1/2 flex-col flex gap-y-3 justify-center items-center mx-auto">
      <h2 class="text-3xl font-bold mb-4">What is collaborative Learning</h2>
      <p class="text-gray-600">
        Collaborative learning is an educational approach to teaching and learning that
        involves groups of learners working together to solve a problem, complete a task,
        or create a product. It is based on the idea that learning is a naturally social
        act in which the participants talk among themselves.
      </p>
      <a
        href="#"
        class="bg-black text-white py-2 px-4 rounded hover:bg-black mt-4 flex justify-center justify-self-center items-center mx-auto"
        >Learn More</a
      >
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
