<script setup>
const emit = defineEmits(['filter']);

const filter = (e) => {
  //console.log(e.target.value)
  emit('filter', e.target.value);
};

</script>

<template>
  <form class="px-4 py-3 flex items-center">
    <label class="sr-only">Search</label>
    <div class="relative w-full">
      <input type="text" placeholder="Search" @input="filter" class="px-2 py-2 bg-gray-50 rounded border border-gray-300 text-gray-900 text-sm" />
    </div>
  </form>
</template>
