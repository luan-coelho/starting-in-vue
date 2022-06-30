<template>
  <FormUser />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import FormUser from '@/components/FormUser.vue';

export default defineComponent({
  name: 'App',
  components: { FormUser }
});
</script>

<style></style>
