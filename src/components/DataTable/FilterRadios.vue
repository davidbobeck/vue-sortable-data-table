<script setup>
const emit = defineEmits(['filter']);

const filter = (e) => {
  emit('filter', e.target.value);
};
</script>

<template>
  <div class="flex flex-row items-center w-auto">
    <label class="mr-4 flex items-center w-24">
      <input type="radio" name="filter-radios" value="all" checked @change="filter" />
      <span class="ml-1">Show All</span>
    </label>
    <label class="mr-4 flex items-center w-24">
      <input type="radio" name="filter-radios" value="today" @change="filter" />
      <span class="ml-1">Due Today</span>
    </label>
    <label class="mr-4 flex items-center w-24">
      <input type="radio" name="filter-radios" value="past" @change="filter" />
      <span class="ml-1">Past Due</span>
    </label>
  </div>
</template>
