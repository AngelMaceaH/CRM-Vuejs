<template>
  <div>
    <div class="flex justify-end">
      <RouterLink to="listado-clientes"> Volver</RouterLink>
    </div>
    <Heading>{{ titulo }}</Heading>
    <div class="mx-auto mt-10 bg-white shadow">
      <div class="mx-auto md:w-2/3 py-20 px-6">
        <FormKit type="form" submit-label="Agregar Cliente" incomplete-message="No se pudo enviar, revisa los mensajes"
          @submit="handleSubmit">
          <FormKit type="text" label="Nombre" name="nombre" placeholder="Nombre de cliente" validation="required"
            :validation-messages="{ required: 'El nombre del clientes es obligatorio' }" />
          <FormKit type="text" label="Apellido" name="apellido" placeholder="Apellido de cliente" validation="required"
            :validation-messages="{ required: 'El apellido del clientes es obligatorio' }" />
          <FormKit type="email" label="Email" name="email" placeholder="Email de cliente" validation="required|email"
            :validation-messages="{
              required: 'El email del clientes es obligatorio',
              email: 'Coloca un email valido',
            }" />
          <FormKit type="text" label="Teléfono" name="telefono" placeholder="Teléfono XXX-XXX-XXXX"
            validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
            :validation-messages="{ matches: 'El formato no es válido' }" />
          <FormKit type="text" label="Empresa" name="empresa" placeholder="Empresa de cliente" />
          <FormKit type="text" label="Puesto" name="puesto" placeholder="Puesto de cliente" />
        </FormKit>
      </div>
    </div>
  </div>
</template>

<script setup>
import ClienteService from "../services/ClienteService.js"
import RouterLink from '../components/UI/RouterLink.vue'
import Heading from '../components/UI/Heading.vue'
import { FormKit } from '@formkit/vue'
import { useRouter } from 'vue-router'
const router = useRouter()
defineProps({
  titulo: {
    type: String,
  },
})
const handleSubmit = (data) => {
  data.estado = 1
  ClienteService.agregarCliente(data)
    .then((respuesta) => {
      router.push({ name: 'listado-clientes' })
    })
    .catch((error) => console.log(error))
}
</script>

<style>
.formkit-wrapper {
  max-width: 100%;
}
</style>