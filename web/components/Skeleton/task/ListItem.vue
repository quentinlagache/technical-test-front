<template functional>
  <div
    class="flex justify-between px-4 py-4 sm:px-6 animate-pulse"
    :style="{ animationDelay: index === 2 ? '600ms': index === 3 ? '1200ms': ''}"
  >
    <div class="w-full">
      <div class="flex space-x-4">
        <div class="w-20 h-5 bg-gray-200 rounded-full" />
        <div class="flex-1 max-w-sm py-1 space-y-6">
          <div class="h-3 bg-gray-200 rounded" />
        </div>
      </div>
      <div class="max-w-sm mt-4 space-y-3">
        <div class="h-2 mr-8 bg-gray-200 rounded" />
        <div class="grid grid-cols-3 gap-4">
          <div class="h-2 col-span-2 bg-gray-200 rounded" />
          <div class="h-2 col-span-1 bg-gray-200 rounded" />
        </div>
      </div>
    </div>
    <div class="flex space-x-2.5 ml-2.5">
      <div class="w-5 h-5 bg-gray-200 rounded" />
      <div class="w-5 h-5 bg-gray-200 rounded" />
    </div>
  </div>
</template>
