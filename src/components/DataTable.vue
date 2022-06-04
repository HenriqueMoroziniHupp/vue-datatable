<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  data: Array,
  columns: Array,
  filterKey: String
})

const sortKey = ref('')
const sortOrders = ref(props.columns.reduce((o, key) => ((o[key] = 1), o), {}))

const filteredData = computed(() => {
  let { data, filterKey } = props
  if (filterKey) {
    filterKey = filterKey.toLowerCase()
    data = data.filter((row) => {
      return Object.keys(row).some((key) => {
        return String(row[key]).toLowerCase().indexOf(filterKey) > -1
      })
    })
  }
  const key = sortKey.value
  if (key) {
    const order = sortOrders.value[key]
    data = data.slice().sort((a, b) => {
      a = a[key]
      b = b[key]
      return (a === b ? 0 : a > b ? 1 : -1) * order
    })
  }
  return data
})

function sortBy(key) {
  sortKey.value = key
  sortOrders.value[key] *= -1
}

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1)
}
</script>

<template>
  <div class="container-table">
    <table class="data-table" v-if="filteredData.length">
      <thead>
        <tr>
          <th
            v-for="key in columns"
            @click="sortBy(key)"
            :class="{ active: sortKey == key }"
            :key="columns[key]"
          >
            {{ capitalize(key) }}
            <span class="arrow" :class="sortOrders[key] > 0 ? 'asc' : 'dsc'">
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="entry in filteredData">
          <td v-for="key in columns">
            {{ entry[key] }}
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>Registro não encontrado</p>
  </div>
</template>

<style>
.data-table {
  width: max-content;
  height: max-content;
}

table {
  /* border: 2px solid #42b983; */
  border-radius: 3px;
  background-color: #f4f4f6;
}

th {
  background-color: #42b983;
  color: rgba(255, 255, 255, 0.66);
  cursor: pointer;
}

td {
  background-color: #f9f9f9;
}

th,
td {
  text-align: center;
  font-size: 0.9rem;
  min-width: 150px;
  padding: 5px;
}

th.active {
  color: #fff;
}

th.active .arrow {
  opacity: 1;
}

.rows {
  display: grid;
  height: 300px;
  overflow-x: scroll;
}
.arrow {
  display: inline-block;
  vertical-align: middle;
  width: 0;
  height: 0;
  margin-left: 5px;
  opacity: 0.66;
}

.arrow.asc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-bottom: 4px solid #fff;
}

.arrow.dsc {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid #fff;
}
</style>
