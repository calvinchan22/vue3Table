<script setup>
import CyhTable from "./components/table/table.vue";
import {testColumns, testData} from "./components/table/tableTestData.js";
import {onMounted, ref} from "vue";

let columns = ref([]);
let data = ref([]);
let pageNo = ref(1);
let pageSize = ref(10);

function _seeDetail (row, index) {
  const {name, age, hobby} = row;
  window.alert(`我是${name}，我的序号是${index + 1}，我今年${age}岁，我喜欢${hobby}`)
}

function onTableRowClick(row, index) {
  window.alert(`点击了第${index + 1}行，其内容为${row}`)
}

onMounted(() => {
  columns.value = testColumns;
  data.value = testData;
})

</script>

<template>
  <cyh-table :columns="columns"
             :data="data"
             :pagination="{
               total: data.length,
               pageNo: pageNo,
               pageSize: pageSize
             }"
             @row-click="onTableRowClick">
    <template #opr="{row, data, index}" >
      <span class="link-opr" @click="_seeDetail(row, index)">查看详情</span>
    </template>
  </cyh-table>
</template>



<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;
}

.link-opr {
  color: #3366ff;

  &:hover {
    cursor: pointer;
    color: blue;
  }
}
</style>
