<script setup>
import { ref } from 'vue'
import { obtenerToken } from '../services/api.js'

// Evento que este componente le envía a App.vue
const emit = defineEmits(['sesion-iniciada'])

const correo = ref('')
const licencia = ref('')
const mensaje = ref('')

async function iniciarSesion() {
  try {
    const respuesta = await obtenerToken(correo.value, licencia.value)
    localStorage.setItem('token', respuesta.token)
    mensaje.value = 'Sesión iniciada. Token guardado.'
    emit('sesion-iniciada', respuesta.token)
  } catch (error) {
    mensaje.value = 'Error: ' + error.message
  }
}
</script>

<template>
  <div class="login-card">
    <h1>Acceso a la API</h1>
    <p class="subtitulo">Ingresa tu correo y tu licencia</p>

    <form @submit.prevent="iniciarSesion">
      <label>Correo</label>
      <input v-model="correo" type="email" required />

      <label>Licencia</label>
      <input v-model="licencia" type="password" required />

      <button type="submit" class="btn-principal">Obtener token</button>
    </form>

    <p v-if="mensaje" class="mensaje" :class="{ error: mensaje.startsWith('Error') }">
      {{ mensaje }}
    </p>
  </div>
</template>

<style scoped>
.login-card {
  width: 360px;
  background: #1e1e26;
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  padding: 32px;
  box-sizing: border-box;
}

h1 {
  color: #fff;
  margin: 0 0 4px 0;
  font-size: 22px;
}

.subtitulo {
  color: #8e8e93;
  font-size: 14px;
  margin: 0 0 24px 0;
}

form {
  display: flex;
  flex-direction: column;
}

label {
  color: #8e8e93;
  font-size: 12px;
  margin-bottom: 6px;
}

input {
  background: #2a2a35;
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #fff;
  padding: 12px;
  border-radius: 10px;
  font-size: 14px;
  margin-bottom: 16px;
  outline: none;
}

input:focus {
  border-color: #a634ff;
}

button {
  width: 100%;
  padding: 12px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
}

.btn-principal {
  background: linear-gradient(135deg, #a634ff, #7a12ff);
  color: #fff;
  border: none;
  margin-top: 4px;
}

.mensaje {
  margin: 16px 0 0 0;
  padding: 10px 12px;
  border-radius: 8px;
  font-size: 13px;
  background: rgba(56, 184, 158, 0.12);
  color: #38b89e;
}

.mensaje.error {
  background: rgba(255, 77, 77, 0.1);
  color: #ff4d4d;
}

@media (max-width: 530px) {
  .login-card {
    width: calc(100% - 32px);
    padding: 22px;
  }

  input,
  button {
    min-height: 44px;
    font-size: 16px;
  }
}
</style>