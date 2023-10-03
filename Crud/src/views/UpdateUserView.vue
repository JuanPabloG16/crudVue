

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';

const router = useRouter();
const route = useRoute();
const userId = route.params.id;

const user = ref(null);

onMounted(async () => {
  await getUser();
});

const getUser = async () => {
  const response = await fetch(`https://fakestoreapi.com/users/${userId}`);
  user.value = await response.json();
};

const updateUser = async () => {
  const response = await fetch(`https://fakestoreapi.com/users/${userId}`, {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(user.value),
  });
  
  if (response.ok) {
    router.push('/users');
  }
};
</script>

<template>
  <main>
    <h1>Actualizar usuario</h1>
    
    <form @submit.prevent="updateUser" v-if="user">
      <input v-model="user.name.firstname" placeholder="Nombre" required />
      <input v-model="user.name.lastname" placeholder="Apellido" required />
      <input v-model="user.email" placeholder="Email" required />
      <input v-model="user.username" placeholder="Nombre de usuario" required />
      <input v-model="user.password" placeholder="Contraseña" required />
    
      <button type="submit">Actualizar usuario</button>
    </form>
  </main>
</template>
