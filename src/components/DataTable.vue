<template>
  <h2>Users</h2>
  <div v-for="user in users" v-bind:key="user.id">{{ user.name }}</div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from '@/utils/axios';
import { User } from '@/models/User';

export default defineComponent({
  data() {
    return {
      users: [] as User[]
    };
  },
  created() {
    this.getAll();
  },
  methods: {
    async getAll() {
      await axios
        .get('/users')
        .then((response) => {
          this.users = response.data;
        })
        .catch((error) => {
          console.warn(error);
        });
    }
  }
});
</script>

<style scoped>
* {
  color: #fff;
}
</style>
