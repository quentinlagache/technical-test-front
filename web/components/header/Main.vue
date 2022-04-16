<template>
  <header class="bg-indigo-600">
    <div class="container flex items-center justify-between h-20 px-4 mx-auto text-white sm:px-0">
      <NuxtLink :to="{ name: 'index' }" class="inline-flex items-center font-medium gap-x-5">
        <SvgIcon name="icons/logo" class="w-10 h-10" />
        Task editor
      </NuxtLink>
      <NuxtLink v-if="$nuxt.$route.name === 'index'" :to="{ name: 'tasks-add' }" class="px-4 py-2 font-medium text-indigo-600 bg-white rounded-md">
        Add
      </NuxtLink>
    </div>
  </header>
</template>
