<script setup>
import { ref } from 'vue'
import LoginForm from './components/LoginForm.vue'
import PerfilUsuario from './components/PerfilUsuario.vue'

// Lee el token al abrir la app (puede venir vacío)
const token = ref(localStorage.getItem('token'))

// El formulario avisa que ya hay token
function alIniciarSesion(nuevoToken) {
  token.value = nuevoToken
}

// El perfil avisa que el usuario quiere salir
function alCerrarSesion() {
  localStorage.removeItem('token')
  token.value = null
}
</script>

<template>
  <PerfilUsuario v-if="token" @sesion-cerrada="alCerrarSesion" />
  <LoginForm v-else @sesion-iniciada="alIniciarSesion" />
</template>

<style>
body {
  margin: 0;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #15151b;
  font-family: 'Segoe UI', system-ui, sans-serif;
}
</style>