<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const users = ref([]);
const newUser = ref({
  email: '',
  username: '',
  password: '',
  name: {
    firstname: '',
    lastname: ''
  },
  address: {
    city: '',
    street: '',
    number: '',
    zipcode: '',
    geolocation: {
      lat: '',
      long: ''
    }
  },
  phone: ''
});

onMounted(async () => {
  await getUsers();
});

const getUsers = async () => {
  const response = await fetch('https://fakestoreapi.com/users');
  users.value = await response.json();
};

const createUser = async () => {
  const response = await fetch('https://fakestoreapi.com/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(newUser.value),
  });
  
  if (response.ok) {
    newUser.value = {
      email: '',
      username: '',
      password: '',
      name: {
        firstname: '',
        lastname: ''
      },
      address: {
        city: '',
        street: '',
        number: '',
        zipcode: '',
        geolocation: {
          lat: '',
          long: ''
        }
      },
      phone: ''
    };
    
    await getUsers();
  }
};

const deleteUser = async (user) => {
  const response = await fetch(`https://fakestoreapi.com/users/${user.id}`, {
    method:'DELETE'
  });
  
  if (response.ok) {
    await getUsers();
  }
};

const goToUpdateUser = (user) => {
  router.push(`/update-user/${user.id}`);
};
</script>

<template>
  <main>
    <h1>Hola desde crud</h1>
    
    <form @submit.prevent="createUser">
      <input v-model="newUser.name.firstname" placeholder="Nombre" required />
      <input v-model="newUser.name.lastname" placeholder="Apellido" required />
      <input v-model="newUser.email" placeholder="Email" required />
      <input v-model="newUser.username" placeholder="Nombre de usuario" required />
      <input v-model="newUser.password" placeholder="Contraseña" required />
      <button type="submit">Crear usuario</button>
    </form>

    <div v-for="user in users" :key="user.id">
      <p>{{ user.name.firstname }} {{ user.name.lastname }}</p>
      <button @click="goToUpdateUser(user)">Actualizar</button>
      <button @click="deleteUser(user)">Eliminar</button>
    </div>
  </main>
</template>

