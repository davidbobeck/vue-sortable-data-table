<script setup>
import { onMounted, ref } from "vue";
import DataTable from "./components/DataTable.vue";

const items = ref([]);

// onMounted(async () => {
//   const response = await fetch('http://localhost:8000/api/tasks');
//   items.value = await response.json();
// }
onMounted(() => {
  items.value = [
    {
      id: 1,
      name: 'David',
      status: 'Sleeping',
      title: 'Sleeping with giants',
      due_at: "2025-06-05"
    },
    {
      id: 2,
      name: 'Laura',
      status: 'Sleeping',
      title: 'Sleeping on the couch',
      due_at: "2024-05-03"
    },
    {
      id: 3,
      name: 'Henry',
      status: 'Reading',
      title: 'Number two son',
      due_at: "2024-05-28"
    },
    {
      id: 4,
      name: 'Hunter',
      status: 'Eating',
      title: 'Number one son',
      due_at: "2026-05-05"
    },
    {
      id: 5,
      name: 'Victoria',
      status: 'Cooking',
      title: 'Yum yum, eatem up',
      due_at: "2024-05-05"
    }
  ]
});

</script>

<template>
  <div class="text-3xl font-bold p-8 bg-gray-100 min-height-screen">
    <DataTable :items="items">
    </DataTable>
  </div>
</template>

<style scoped>
</style>
