<template>
  <table>
    <thead>
      <th v-for="(headTdItem, headTdIndex) in columns" :key="`head-td-index${headTdIndex}`">
          {{headTdItem.name}}
      </th>
    </thead>
    <tbody>
      <tr v-for="(bodyTrItem, bodyTrIndex) in data" :key="`body-tr-index${bodyTrIndex}`">
        <td v-for="(bodyTdItem, bodyTdIndex) in columns" :key="`body-td-index${bodyTdIndex}`">
          {{bodyTrItem[bodyTdItem.name]}}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>

import {defineProps} from "vue";

const props = defineProps({
  columns: {
    type: Array,
    default: () => []
  },

  data: {
    type: Array,
    default: () => []
  }
})
</script>