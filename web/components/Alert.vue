<template functional>
  <div class="p-4 rounded-md " :class="props.type === 'success' ? 'bg-green-100' : 'bg-red-50'">
    <p
      class="text-sm font-medium"
      :class="props.type === 'success' ? 'text-green-800' : 'text-red-800'"
    >
      <slot />
    </p>
  </div>
</template>
