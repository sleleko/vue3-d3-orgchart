<template>
  <div id="app">
    <OrgChart :data="orgData" :width="width" :height="height" />
  </div>
</template>

<script setup>
import OrgChart from './components/OrgChart.vue'

const width = 932
const height = 500

const orgData = {
  name: 'CEO',
  contact: 'ceo@example.com',
  children: [
    {
      name: 'CTO',
      contact: 'cto@example.com',
      children: [
        { name: 'Engineer1', contact: 'eng1@example.com' },
        { name: 'Engineer2', contact: 'eng2@example.com' }
      ]
    },
    {
      name: 'CFO',
      contact: 'cfo@example.com'
    },
    {
      name: 'CMO',
      contact: 'cmo@example.com',
      children: [
        { name: 'Marketing1', contact: 'mkt1@example.com' }
      ]
    }
  ]
};
</script>

<style>
#app {
  background-color: white;
}
</style>
