<script setup>
import { ref } from 'vue';
import axios from 'axios';

import { useRouter } from 'vue-router';
// Crear referencias reactivas para los campos del formulario
const email = ref('');
const password = ref('');
const router = useRouter();

// Función para manejar el envío del formulario
const login = async () => {
  try {
    // Enviar una solicitud POST al backend con los datos del formulario
    const response = await axios.post('/api/Login', {
      email: email.value,
      password: password.value,
    });

    // Manejar la respuesta
    if (response.status === 200) {
      console.log(response);

      localStorage.setItem('userData', JSON.stringify({
        id:response.data.id,
        userName: response.data.userName,
        rol: response.data.rol,
        email: response.data.email
      }));

      alert(`Bienvenida: ${response.data.userName}`);
      router.push("/dashboard") // Mostrar mensaje de éxito
      
    } else {
      alert('Acceso denegado: ' + response.data); // Mostrar mensaje de error
    }
  } catch (error) {
    console.error('Error al iniciar sesión:', error);
    alert(error.response.data);
  }
};
</script>

<template>
  <form @submit.prevent="login" class="bg-white block w-2/4 mx-auto my-64 rounded-md p-5">
    <h1 class="text-center">Iniciar Sesión</h1>
    <div class="grid grid-cols-1 w-50 p-5">
      <label>Correo Electrónico:</label>
      <input v-model="email" type="text" placeholder="correo@correo.com" class="border rounded-md">
      
      <label>Contraseña:</label>
      <input v-model="password" type="password" placeholder="**********" class="border rounded-md">
    </div>
    <button type="submit" class="border rounded-full bg-zinc-300 px-2 py-1 hover:bg-zinc-500">Iniciar sesión</button>
  </form>
  <p>¿Aún no estás registrado? <router-link to="/registro">Click Aquí</router-link></p>
</template>

<style scoped>
/* Estilos opcionales */
</style>
