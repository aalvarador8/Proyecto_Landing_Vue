<script setup>
import { reactive, ref } from 'vue'

const formulario = reactive({
  nombre: '',
  correo: '',
  servicio: '',
  mensaje: ''
})

const errores = reactive({
  nombre: '',
  correo: '',
  servicio: '',
  mensaje: ''
})

const enviado = ref(false)

const validarFormulario = () => {
  errores.nombre = ''
  errores.correo = ''
  errores.servicio = ''
  errores.mensaje = ''

  let valido = true

  if (!formulario.nombre.trim()) {
    errores.nombre = 'El nombre es obligatorio'
    valido = false
  }

  if (!formulario.correo.trim()) {
    errores.correo = 'El correo es obligatorio'
    valido = false
  } else if (!/\S+@\S+\.\S+/.test(formulario.correo)) {
    errores.correo = 'Ingrese un correo válido'
    valido = false
  }

  if (!formulario.servicio) {
    errores.servicio = 'Seleccione un servicio'
    valido = false
  }

  if (!formulario.mensaje.trim()) {
    errores.mensaje = 'El mensaje es obligatorio'
    valido = false
  }

  return valido
}

const manejarEnvio = () => {
  enviado.value = false

  if (validarFormulario()) {
    enviado.value = true

    formulario.nombre = ''
    formulario.correo = ''
    formulario.servicio = ''
    formulario.mensaje = ''
  }
}
</script>

<template>
  <section class="section" id="contacto">
    <h2>Contacto</h2>

    <form class="form" @submit.prevent="manejarEnvio">
      <input
        type="text"
        placeholder="Ingrese su nombre"
        v-model="formulario.nombre"
      />
      <span v-if="errores.nombre">{{ errores.nombre }}</span>

      <input
        type="email"
        placeholder="Ingrese su correo"
        v-model="formulario.correo"
      />
      <span v-if="errores.correo">{{ errores.correo }}</span>

      <select v-model="formulario.servicio">
        <option value="">Seleccione un servicio</option>
        <option value="automatizacion">Automatización</option>
        <option value="datos">Análisis de datos</option>
        <option value="prediccion">Modelos predictivos</option>
      </select>
      <span v-if="errores.servicio">{{ errores.servicio }}</span>

      <textarea
        placeholder="Escriba su mensaje"
        v-model="formulario.mensaje"
      ></textarea>
      <span v-if="errores.mensaje">{{ errores.mensaje }}</span>

      <button type="submit">Enviar</button>

      <p v-if="enviado" class="success">Formulario enviado correctamente.</p>
    </form>
  </section>
</template>