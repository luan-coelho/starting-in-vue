<template>
  <div class="users">
    <div class="container">
      <section>
        <h5 class="title">Novo Usuário</h5>
        <form @submit.prevent="createUser()">
          <input
            class="form-field"
            type="text"
            v-model="user.name"
            placeholder="Nome"
          />
          <input
            class="form-field"
            type="text"
            v-model="user.email"
            placeholder="Email"
          />
          <button
            class="button-create"
            :disabled="user.name == '' || user.email == ''"
            type="submit"
          >
            Criar
          </button>
        </form>
      </section>

      <section v-if="users.length > 0">
        <h5 class="title">Lista de Usuários</h5>
        <ul>
          <li v-for="userbd in users" :key="userbd.id">
            <p>{{ userbd.name }}</p>
            <small>{{ userbd.email }}</small>
            <div class="buttons">
              <label
                >{{ userbd.ativo ? 'Ativar' : 'Desativar' }}
                <input
                  id="ativo"
                  @click="activateOrDeactivate(userbd.id, userbd.ativo)"
                  type="checkbox"
                  v-model="userbd.ativo"
                />
              </label>
              <a @click="deleteUser(userbd.id)">Deletar</a>
            </div>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from '@/utils/axios';
import { User } from '@/models/User';

export default defineComponent({
  data() {
    return {
      users: [] as User[],
      user: {
        name: '',
        email: '',
        ativo: true
      }
    };
  },
  created() {
    this.getAllUsers();
  },
  methods: {
    async getAllUsers() {
      await axios
        .get('/users')
        .then((response) => {
          this.users = response.data;
        })
        .catch((error) => {
          console.warn(error);
        });
    },
    async createUser() {
      await axios
        .post('/users', this.user)
        .then((response) => {
          this.users.push(response.data);
        })
        .catch((error) => {
          console.warn(error);
        });

      this.user.name = '';
      this.user.email = '';
    },
    async activateOrDeactivate(id: number, ativo: boolean) {
      await axios
        .patch(`/users/${id}`, ativo)
        .then(() => {
          this.getAllUsers();
        })
        .catch((error) => {
          console.warn(error);
        });
    },
    async deleteUser(id: number) {
      await axios
        .delete(`/users/${id}`)
        .then(() => {
          this.getAllUsers();
        })
        .catch((error) => {
          console.warn(error);
        });
    }
  },
  computed: {
    filterActiveUsers(): User[] {
      return this.users.filter((user) => user.ativo);
    }
  }
});
('');
</script>

<style scoped>
.container {
  margin: 4rem auto;
  max-width: 500px;
  width: 90%;
  display: grid;
  grid-gap: 2.5rem;
}

.title {
  font-size: 1rem;
  font-weight: 500;
  margin: 0.7rem 0;
}

form {
  display: grid;
  grid-gap: 1rem;
}

.form-field {
  background: transparent;
  border: 1px solid #999fc6;
  border-radius: 1rem;
  padding: 0.6rem;
  outline: none;
  color: #e1e8ef;
}

.form-field::placeholder {
  color: #999fc6;
}

.button-create {
  background-color: #2d6cea;
  color: #e1e8ef;
  border: none;
  border-radius: 1rem;
  padding: 0.6rem 1.5rem;
  width: max-content;
  transition: all 0.3s linear;
  outline: none;
  cursor: pointer;
  box-shadow: 0 0 5px 3px rgba(45, 108, 234, 0.3);
}

.button-create:hover {
  background-color: #1b5cdc;
}

p {
  margin: 0;
}

ul {
  padding: 0;
  margin: 0;
  display: grid;
  grid-gap: 1rem;
}

li {
  background-color: #2b3a4e;
  padding: 1.2rem 1rem;
  border-radius: 1rem;
  position: relative;
  list-style: none;
  color: #8b98a8;
}

.buttons {
  display: flex;
  align-items: center;
  justify-items: center;
}

.buttons:first-child {
  width: 50px;
  margin-right: 30px;
}

.buttons label {
  font-weight: 700;
}

.buttons input {
  margin-top: 15px;
  height: 1.5rem;
  width: calc(2rem + 0.75rem);
  border-radius: 3rem;
}
</style>
