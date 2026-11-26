<script setup>
import { ref, onMounted } from 'vue'
import { obtenerPerfil } from '../services/api.js'

// Evento que este componente le envía a App.vue
const emit = defineEmits(['sesion-cerrada'])

const perfil = ref(null)
const cargando = ref(true)
const error = ref('')

async function cargarPerfil() {
  cargando.value = true
  error.value = ''

  try {
    perfil.value = await obtenerPerfil()
    console.log(perfil.value)
  } catch (e) {
    error.value = e.message
  } finally {
    cargando.value = false
  }
}

function cerrarSesion() {
  emit('sesion-cerrada')
}

onMounted(cargarPerfil)
</script>

<template>
  <div class="perfil-card">
    <h1>Mi perfil</h1>
    <p class="subtitulo">Datos de tu cuenta en la API</p>

    <p v-if="cargando" class="estado">Cargando perfil...</p>

    <p v-else-if="error" class="mensaje error">Error: {{ error }}</p>

    <div v-else-if="perfil" class="datos">
      <p><strong>Nombre:</strong> {{ perfil.first_name }} {{ perfil.last_name }}</p>
      <p><strong>Correo:</strong> {{ perfil.email }}</p>
      <p><strong>Escuela:</strong> {{ perfil.school }}</p>
      <p><strong>Items:</strong> {{ perfil.items_count }} de {{ perfil.items_limit }}</p>
    </div>

    <button @click="cargarPerfil" :disabled="cargando" class="btn-principal">Actualizar</button>
    <button @click="cerrarSesion" class="btn-secundario">Cerrar sesión</button>
  </div>
</template>

<style scoped>
.perfil-card {
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

.estado {
  color: #8e8e93;
  font-size: 14px;
  margin: 0 0 16px 0;
}

.datos {
  background: #2a2a35;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  padding: 4px 14px;
  margin-bottom: 16px;
}

.datos p {
  color: #fff;
  font-size: 14px;
  margin: 10px 0;
}

.datos strong {
  color: #8e8e93;
  font-weight: 500;
  margin-right: 4px;
}

button {
  width: 100%;
  padding: 12px;
  border-radius: 10px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-principal {
  background: linear-gradient(135deg, #a634ff, #7a12ff);
  color: #fff;
  border: none;
}

.btn-secundario {
  background: #2a2a35;
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 10px;
}

.mensaje {
  margin: 0 0 16px 0;
  padding: 10px 12px;
  border-radius: 8px;
  font-size: 13px;
}

.mensaje.error {
  background: rgba(255, 77, 77, 0.1);
  color: #ff4d4d;
}

@media (max-width: 530px) {
  .perfil-card {
    width: calc(100% - 32px);
    padding: 22px;
  }

  button {
    min-height: 44px;
    font-size: 16px;
  }
}
</style>