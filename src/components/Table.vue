<script>
export default {
  props: {
    headers: Array,
  },
};
</script>

<template>
  <table class="border text-sm text-left w-full">
    <thead class="flex w-full bg-gray-100 text-gray-500 p-4">
      <tr class="flex w-full my-2">
        <th v-for="header in headers" :key="header" class="w-full">
          {{ header }}
        </th>
      </tr>
    </thead>
    <tbody
      class="flex flex-col items-center justify-between overflow-y-scroll w-full"
      style="height: 50vh"
    >
      <slot />
    </tbody>
  </table>
</template>
