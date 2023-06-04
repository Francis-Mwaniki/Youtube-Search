<template>
  <!-- register svg  and button-->
  <div class="flex flex-col items-end justify-end py-1">
    <!--h2 Our Courses and how are served -->

    <div class="flex flex-col items-center justify-center">
      <div class="flex flex-col items-center justify-center">
        <p class="mt-2 text-sm text-gray-600 px-4">
          <NuxtLink
            to="/Register"
            class="font-medium text-indigo-600 hover:text-indigo-500"
          >
            Register
          </NuxtLink>
          <span>or</span>
          <NuxtLink to="/Login" class="font-medium text-indigo-600 hover:text-indigo-500">
            Sign in to your account
          </NuxtLink>
        </p>
      </div>
    </div>
  </div>

  <div class="bg-gray-100 py-16 mb-9">
    <h2 class="text-lg font-bold mb-4 text-center mt-4 text-black">
      OUR MOST LOVED COURSES
    </h2>
    <div
      class="container mx-auto flex flex-col lg:flex-row items-center justify-center lg:justify-between"
    >
      <!-- v-for loop to render videos from supabase -->

      <Display />
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped></style>
