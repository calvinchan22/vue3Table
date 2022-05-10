<template>
  <div style="width: 100%">
    <table class="simple-table" align="center">
      <thead class="simple-table__head">
      <tr class="simple-table__head-tr">
        <th class="simple-table__head-tr-th"
            v-for="(item, index) in columns"
            :key="`table-thead-${tableUuid}-${index}`">
          {{item.title}}
          <span class="simple-table__head-tr-th-sort" v-if="item.sortAble">
            <span @click="_onSort(item, 'ASC')"
                  :class="_getSortBtnActiveClass(item, 'ASC')">
              up
            </span>
            <span @click="_onSort(item, 'DESC')"
                  :class="_getSortBtnActiveClass(item, 'DESC')">
              down
            </span>
          </span>
        </th>
      </tr>
      </thead>
      <tbody class="simple-table__body">
      <tr v-for="(item, index) in showTableData"
          class="simple-table__body-tr"
          @click="_onRowClick(item, index)"
          :key="`table-tbody-tr-${tableUuid}-${index}`">
        <td v-for="(v, k) in tableColumns"
            class="simple-table__body-tr-td"
            :key="`table-tbody-td-${tableUuid}-${index}-${k}`">
          <template v-if="!v.scopedSlots.customRender">{{ item[v.key] }}</template>
          <!-- 自定义渲染 -->
          <template v-else>
            <slot :name="v.key"
                  :index="index"
                  :row="item"
                  :data="item[v.key]"
            ></slot>
          </template>
        </td>
      </tr>
      </tbody>
    </table>
    <cyh-pagination :total="pagination.total"
                    :page-size="pagination.pageSize"
                    :page-no="pagination.pageNo"
                    @page-click="pageClick">
    </cyh-pagination>
  </div>
</template>

<script setup>
import {compare} from '../../util/index.js';
import {v4} from 'uuid';
import {computed, ref, watch, defineProps} from "vue";
import CyhPagination from '../pagination/index.vue';
const props = defineProps({
  // 表格头的数据
  columns: {
    type: Array,
    default: () => []
  },

  // 表格的数据
  data: {
    type: Array,
    default: () => []
  },

  pagination: {
    type: Object,
    default: () => {
      return {
        total: 0,
        pageSize: 0,
        pageNo: 0
      }
    }
  }
});
const tableUuid = v4();
const tableColumns = ref([]);
const tableData = ref([]);
const pagination = ref({});

const showTableData = computed(() => {
  const start = (pagination.value.pageNo - 1) * pagination.value.pageSize;
  return tableData.value.slice(start, start + pagination.value.pageSize);
})

watch(
    () => props.data,
    (newValue) => {
      tableData.value = [...newValue];
    }
)

watch(
    () => props.columns,
    (newValue) => {
      tableColumns.value = [...newValue];
    }
)

watch(
    () => props.pagination,
    (newValue) => {
      pagination.value = {...newValue};
    }
)

const sortRecord = ref({
  key: '',
  direc: ''
});

const emit = defineEmits(['rowClick']);
function _onRowClick (item, index) {
  emit('rowClick',item, index);
}

function pageClick (index) {
  pagination.value.pageNo = index + 1;
}

function _getSortFn (key, direc) {
  const sortFn = (record0, record1, direc) => {
    if (direc === 'ASC') {
      return compare(record0[key], record1[key]);
    }
    return compare(record1[key], record0[key]);
  };

  return (record0, record1) => sortFn(record0, record1, direc);
}

function _onSort (item, direc) {
  const key = item.key;

  if (sortRecord.value.key === key && sortRecord.value.direc === direc) {
    tableData.value = [...props.data];
    sortRecord.value = {
      key: '',
      direc: ''
    }
    return;
  }

  const sortFn = _getSortFn(key, direc);

  tableData.value.sort(sortFn);

  sortRecord.value = {
    key: item.key,
    direc: direc
  }
}

function _getSortBtnActiveClass (column, direc) {
  return (sortRecord.value.key === column.key && sortRecord.value.direc === direc) ? 'simple-table__head-tr-th-sort--active' : ''
}
</script>

<style lang="less">

.simple-table {
  width: 100%;
}

</style>