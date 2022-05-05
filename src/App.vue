<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import SimpleTable from "./components/table.vue";

const tableColumns = [{
  name: 'test1'
},{
  name: 'test2'
},{
  name: 'test3'
}]

const data = [{
  test1: 'cpx',
  test2: 'cyh',
  test3: 'lxy'
},{
  test1: 'cpx',
  test2: 'cyh',
  test3: 'lxy'
},{
  test1: 'cpx',
  test2: 'cyh',
  test3: 'lxy'
}]
</script>

<template>
  <SimpleTable :columns="tableColumns" :data="data"/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;
}
</style>
