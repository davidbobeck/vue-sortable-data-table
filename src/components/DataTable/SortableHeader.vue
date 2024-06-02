<script setup>

const props = defineProps({
  column: {
    type: String,
    required: true
  },
  sortColumn: {
    type: String,
    required: true
  },
  sortDescending: {
    type: Boolean,
    required: false,
    default: false
  }
})

const emit = defineEmits(['sortByColumn']);

const sortByColumn = () => {
  console.log(`SortableHeader: sortByColumn ${props.column}`)
  emit('sortByColumn', props.column)
};

</script>

<template>
  <th class="px-4 py-3" @click="sortByColumn">
    <slot></slot>
    <span v-if="props.sortColumn == props.column && !props.sortDescending">
      <i class="fa fa-fw fa-solid fa-sort-up"></i>
    </span>
    <span v-if="props.sortColumn == props.column && props.sortDescending">
      <i class="fa fa-fw fa-solid fa-sort-down"></i>
    </span>
  </th>
</template>
