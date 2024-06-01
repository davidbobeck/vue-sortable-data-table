<script setup>
import { computed, ref } from "vue";

const emit = defineEmits(['filter']);

const filter = (e) => {
  //console.log(e.target.value)
  emit('filter', e.target.value);
};

const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})

const show = ref(false);

const toggleShow = () => {
  show.value = !show.value;
};

const statuses = computed(() => {
  return [...new Set(props.items.map(item => item.status))];
  // return ["abc", "def", "xyz"];
});

</script>

<template>
  <div class="relative flex items-center w-full px-4">
    <button @click="toggleShow"
      class="w-full flex items-center justify-center py-2 px-4 text-sm font-medium text-gray-900 border border-gray-700 rounded">
      Filter
    </button>
    <div v-if="show" class="absolute top-12 right-8 z-10 w-48 p-3 bg-white rounded-lg shadow border border-gray-800">
      <h6 class="mb-3 text-sm font-medium text-gray-900">Status</h6>
      <ul class="space-y-2 text-sm">
        <li v-for="(status, index) in statuses" :key="index">
          <input :id="`filter_option_${index}`" type="checkbox" :value="status" @change="filter" class="w-4 h-4 bg-gray-800">
          <label :for="`filter_option_${index}`" class="ml-2 text-sm font-medium text-gray-900">{{ status }}</label>
        </li>
      </ul>
    </div>
  </div>
</template>
