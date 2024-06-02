<script setup>
import SearchForm from "../DataTable/SearchForm.vue";
import FilterRadios from "../DataTable/FilterRadios.vue";
import FilterDropdown from "../DataTable/FilterDropdown.vue";
import SortableHeader from "../DataTable/SortableHeader.vue";
import { computed, ref } from "vue";

const searchSetting = ref("");
const radioSetting = ref("");
const statusSetting = ref([]);
const sortColumn = ref("id");
const sortDescending = ref(false);

const props = defineProps({
  items: {
    type: Array,
    required: true
  }
})

const filteredItems = computed(() => {
  let items = props.items;

  // SEARCH BOX
  if (searchSetting.value != '') {
    items = items.filter(item => item.title.toLowerCase().includes(searchSetting.value.toLowerCase()) || item.name.toLowerCase().includes(searchSetting.value.toLowerCase()));
  }

  // RADIO BUTTONS
  if (radioSetting.value == 'past') {
    items = items.filter(item => new Date(item.due_at) < new Date());
  } else if (radioSetting.value == 'today') {
    items = items.filter(item => new Date(item.due_at) == new Date());
  }

  // STATUS DROPDOWN
  if (statusSetting.value.length > 0) {
    items = items.filter(item => statusSetting.value.includes(item.status))
  }

  // COLUMN SORTING
  switch (sortColumn.value) {
    case "id":
      if (sortDescending.value) {
        items.sort((a, b) => a.id > b.id ? -1 : 1);
      } else {
        items.sort((a, b) => a.id > b.id ? 1 : -1);
      }
      break;
    case "name":
      if (sortDescending.value) {
        items.sort((a, b) => a.name.toLowerCase() > b.name.toLowerCase() ? -1 : 1);
      } else {
        items.sort((a, b) => a.name.toLowerCase() > b.name.toLowerCase() ? 1 : -1);
      }
      break;
    case "status":
      if (sortDescending.value) {
        items.sort((a, b) => a.status.toLowerCase() > b.status.toLowerCase() ? -1 : 1);
      } else {
        items.sort((a, b) => a.status.toLowerCase() > b.status.toLowerCase() ? 1 : -1);
      }
      break;
    case "title":
      if (sortDescending.value) {
        items.sort((a, b) => a.title.toLowerCase() > b.title.toLowerCase() ? -1 : 1);
      } else {
        items.sort((a, b) => a.title.toLowerCase() > b.title.toLowerCase() ? 1 : -1);
      }
      break;
    case "due_at":
      if (sortDescending.value) {
        items.sort((a, b) => a.due_at > b.due_at ? -1 : 1);
      } else {
        items.sort((a, b) => a.due_at > b.due_at ? 1 : -1);
      }
      break;
  }

  return items;
});

const searchFilter = (search) => {
  searchSetting.value = search;
};

const radioFilter = (filter) => {
  radioSetting.value = filter;
};

const statusFilter = (filter) => {
  if (statusSetting.value.includes(filter)) {
    statusSetting.value = statusSetting.value.filter(e => e !== filter)
  } else {
    statusSetting.value.push(filter);
  }
  console.log(statusSetting.value);
};

// const sortById = () => {
//   console.log("sortById")
//   sortColumn.value = "id";
//   sortDescending.value = !sortDescending.value;
// };

// const sortByName = () => {
//   console.log("sortByName")
//   sortColumn.value = "name";
//   sortDescending.value = !sortDescending.value;
// };

// const sortByStatus = () => {
//   console.log("sortByStatus")
//   sortColumn.value = "status";
//   sortDescending.value = !sortDescending.value;
// };

// const sortByTitle = () => {
//   console.log("sortByTitle")
//   sortColumn.value = "title";
//   sortDescending.value = !sortDescending.value;
// };

// const sortByDueAt = () => {
//   console.log("sortByDueAt")
//   sortColumn.value = "due_at";
//   sortDescending.value = !sortDescending.value;
// };

const sortByColumn = (column) => {
  console.log(`DataTable: sortByColumn ${column}`)
  sortColumn.value = column;
  sortDescending.value = !sortDescending.value;
};

</script>

<template>
  <div class="bg-white relative border rounded-lg">

    <div class="flex items-center justify-between">
      <!-- Search Bar -->
      <SearchForm @filter="searchFilter"></SearchForm>

      <div class="flex items-center justify-end text-sm font-semibold">
        <!-- Radio Buttons -->
        <FilterRadios @filter="radioFilter"></FilterRadios>

        <!-- List of filters for statuses -->
        <FilterDropdown :items="items" @filter="statusFilter"></FilterDropdown>
      </div>
    </div>

    <table class="w-full text-sm text-center text-gray-400">
      <thead class="text-xs text-gray-700 uppercase bg-blue-100 cursor-pointer">
        <tr>
          <SortableHeader column="id" :sortColumn="sortColumn" :sortDescending="sortDescending"
            @sortByColumn="sortByColumn">ID</SortableHeader>
          <SortableHeader column="name" :sortColumn="sortColumn" :sortDescending="sortDescending"
            @sortByColumn="sortByColumn">Name</SortableHeader>
          <SortableHeader column="status" :sortColumn="sortColumn" :sortDescending="sortDescending"
            @sortByColumn="sortByColumn">Status</SortableHeader>
          <SortableHeader column="title" :sortColumn="sortColumn" :sortDescending="sortDescending"
            @sortByColumn="sortByColumn">Title</SortableHeader>
          <SortableHeader column="due_at" :sortColumn="sortColumn" :sortDescending="sortDescending"
            @sortByColumn="sortByColumn">Due At</SortableHeader>
          <!-- <th class="px-4 py-3" @click="sortById">
            ID
            <span v-if="sortColumn == 'id' && !sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-up"></i>
            </span>
            <span v-if="sortColumn == 'id' && sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-down"></i>
            </span>
          </th> -->
          <!-- <th class="px-4 py-3" @click="sortByName">
            Name
            <span v-if="sortColumn == 'name' && !sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-up"></i>
            </span>
            <span v-if="sortColumn == 'name' && sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-down"></i>
            </span>
          </th> -->
          <!-- <th class="px-4 py-3" @click="sortByStatus">
            Status
            <span v-if="sortColumn == 'status' && !sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-up"></i>
            </span>
            <span v-if="sortColumn == 'status' && sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-down"></i>
            </span>
          </th>
          <th class="px-4 py-3" @click="sortByTitle">
            Title
            <span v-if="sortColumn == 'title' && !sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-up"></i>
            </span>
            <span v-if="sortColumn == 'title' && sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-down"></i>
            </span>
          </th>
          <th class="px-4 py-3" @click="sortByDueAt">
            Due At
            <span v-if=" sortColumn=='due_at' && !sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-up"></i>
            </span>
            <span v-if="sortColumn == 'due_at' && sortDescending">
              <i class="fa fa-fw fa-solid fa-sort-down"></i>
            </span>
          </th> -->
          <th class="px-4 py-3">
            <span class="sr-only">Actions</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in filteredItems" :key="item.id" class="border-b">
          <td class="px-4 py-3 font-medium text-gray-900">{{ item.id }}</td>
          <td class="px-4 py-3 font-medium text-gray-900">{{ item.name }}</td>
          <td class="px-4 py-3">{{ item.status }}</td>
          <td class="px-4 py-3">{{ item.title }}</td>
          <td class="px-4 py-3">{{ item.due_at }}</td>
          <td class="px-4 py-3 flex items-enter justify-end">
            <a href="#" class="text-indigo-500 hover:underline">Details</a>
          </td>
        </tr>
      </tbody>
    </table>

  </div>
</template>

<style>
</style>
