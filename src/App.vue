<template>
  <DataTable />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import DataTable from '@/components/DataTable.vue';

export default defineComponent({
  name: 'App',
  components: { DataTable }
});
</script>

<style></style>
