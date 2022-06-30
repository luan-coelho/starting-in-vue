<template>
  <nav>
    <h1>{{ name }}</h1>
    <ul>
      <li>Produtos</li>
      <li>Venda</li>
      <li>Clientes</li>
    </ul>
  </nav>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'NavSideBar',
  data() {
    return {
      name: 'unimacy'
    };
  }
});
</script>

<style scoped></style>
