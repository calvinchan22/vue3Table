<template>
  <div class="pagination">
      <div class="pagination-main">
          <div v-for="(item, index) in pageArr"
               @click="_onPageClick(index)"
               :key="index"
               :class="(index + 1) === props.pageNo ? 'pagination-main__pager--active' : ''"
               class="pagination-main__pager">
              {{index + 1}}
          </div>
      </div>
      <div class="pagination-total">
          总数：{{props.total}}
      </div>
  </div>
</template>

<script setup>
import {defineProps, computed} from "vue";

let props = defineProps({
  total: Number,
  pageNo: Number,
  pageSize: Number
});

const pageArr = computed(() => Math.ceil(props.total / props.pageSize));

const emit = defineEmits(['pageClick']);

function _onPageClick (index) {
  emit('pageClick', index);
}
</script>

<style lang="less">
.pagination {
  display: flex;

  &-main {
    display: flex;
    flex-direction: row;
    flex: 1;

    &__pager {
      width: 20px;
      height: 20px;
      border-radius: 50%;

      &--active {
        background-color: #3366ff;
        color: white;
      }
    }
  }

  &-total {

  }
}
</style>