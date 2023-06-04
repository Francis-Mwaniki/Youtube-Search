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

  <div class="mb-9">
    <Display />
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss" scoped></style>
