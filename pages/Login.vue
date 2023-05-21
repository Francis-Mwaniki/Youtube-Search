<template>
  <div class="mt-6 py-3 m-auto">
    <!-- login form using  tailwindcss -->

    <div class="max-w-md mx-auto my-4 p-4 bg-white rounded-md shadow-md">
      <!-- success alert -->
      <div class="mb-4" v-if="success">
        <div
          class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative"
          role="alert"
        >
          <strong class="font-bold">Success! </strong>
          <span class="block sm:inline">{{ success }}.</span>
        </div>
      </div>
      <!-- error alert -->
      <div class="mb-4" v-if="errMsg">
        <div
          class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative"
          role="alert"
        >
          <strong class="font-bold">Error! </strong>
          <span class="block sm:inline">{{ errMsg }}.</span>
        </div>
      </div>
      <h2 class="text-xl font-medium mb-4">login</h2>
      <Loading v-if="loading" />
      <form @submit.prevent="login">
        <div class="mb-4">
          <label class="block text-gray-700 font-medium mb-2" for="email"> Email </label>
          <input
            class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="email"
            v-model="email"
            type="email"
            placeholder="Enter your email"
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-medium mb-2" for="password">
            Password
          </label>
          <input
            class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="password"
            type="password"
            v-model="password"
            placeholder="Enter your password"
          />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-medium mb-2" for="password-confirm">
            Confirm Password
          </label>
          <input
            class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
            id="password-confirm"
            v-model="passwordConfirm"
            type="password"
            placeholder="Confirm your password"
          />
        </div>
        <button
          class="bg-blue-500 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded focus:outline-none focus:shadow-outline"
          type="submit"
        >
          login
        </button>
      </form>
      <!-- continue github  button-->
      <div class="flex items-center mt-4">
        <hr class="border-gray-300 border-1 w-full rounded-md" />
        <span class="px-2 text-gray-400 text-sm">Or</span>
        <hr class="border-gray-300 border-1 w-full rounded-md" />
      </div>
      <!-- signup with github -->
      <div class="flex items-center mt-4">
        <button
          class="flex items-center justify-center w-full px-4 py-2 text-sm font-medium leading-5 text-white transition-colors duration-150 bg-gray-800 border border-transparent rounded-lg active:bg-gray-800 hover:bg-gray-700 focus:outline-none focus:shadow-outline-gray"
          @click="loginWithGithub"
        >
          <svg
            class="w-4 h-4 mr-3"
            aria-hidden="true"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path
              fill-rule="evenodd"
              d="M10 0a10 10 0 100 20 10 10 0 000-20zm4.5 6a.5.5 0 01.5.5v2.5a.5.5 0 01-1 0V7h-2a.5.5 0 010-1h2V4.5a.5.5 0 01.5-.5zM10 18a7.5 7.5 0 110-15 7.5 7.5 0 010 15zm0-2a5.5 5.5 0 100-11 5.5 5.5 0 000 11z"
              clip-rule="evenodd"
            ></path>
          </svg>
          <span>Sign in with GitHub</span>
        </button>
      </div>
      <p class="mt-4 text-gray-800">
        Don't have an account?
        <NuxtLink to="/" class="text-gray-800 hover:underline"> Sign up </NuxtLink>
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const auth = useSupabaseAuthClient();
let email = ref("");
let password = ref("");
let passwordConfirm = ref("");
const router = useRouter();
let errMsg = ref(null);
let loading = ref(false);
let success = ref("");

const login = async () => {
  if (password.value !== passwordConfirm.value) {
    errMsg.value = "Passwords do not match";
    return;
  }
  try {
    loading.value = true;
    errMsg.value = null;
    const { data, error } = await auth.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    });
    if (error) {
      loading.value = false;
      errMsg.value = "Invalid email or password";
    }
    if (data) {
      success.value = `Welcome ${data.user.email?.split("@")[0]}`;
      setTimeout(() => {
        loading.value = false;
      }, 3000);
      email.value = "";
      password.value = "";
      passwordConfirm.value = "";
      window.location.href = "/Dashboard";
      router.push("/Dashboard");
    }
  } catch (err) {
    loading.value = false;
    errMsg.value = "Invalid email or password";
  }
};
/* signin with github */
const loginWithGithub = async () => {
  const { error } = await auth.auth.signInWithOAuth({
    provider: "github",
  });
  if (error) {
    errMsg.value = error.message;
  } else {
    setTimeout(() => {
      loading.value = false;
    }, 3000);
    email.value = "";
    password.value = "";
    passwordConfirm.value = "";
    router.push("/Dashboard");
  }
};
</script>

<style scoped></style>
