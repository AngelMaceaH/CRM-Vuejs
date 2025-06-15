<template>
    <div>
        <div class="flex justify-end">
            <RouterLink to="listado-clientes"> Volver</RouterLink>
        </div>
        <Heading>{{ titulo }}</Heading>
        <div class="mx-auto mt-10 bg-white shadow">
            <div class="mx-auto md:w-2/3 py-20 px-6">
                <FormKit type="form" submit-label="Editar Cliente"
                    incomplete-message="No se pudo enviar, revisa los mensajes" @submit="handleSubmit"
                    :value="formData">
                    <FormKit type="text" label="Nombre" name="nombre" placeholder="Nombre de cliente"
                        validation="required"
                        :validation-messages="{ required: 'El nombre del clientes es obligatorio' }"
                        v-model="formData.nombre" />
                    <FormKit type="text" label="Apellido" name="apellido" placeholder="Apellido de cliente"
                        validation="required"
                        :validation-messages="{ required: 'El apellido del clientes es obligatorio' }"
                        v-model="formData.apellido" />
                    <FormKit type="email" label="Email" name="email" placeholder="Email de cliente"
                        validation="required|email" :validation-messages="{
                            required: 'El email del clientes es obligatorio',
                            email: 'Coloca un email valido',
                        }" v-model="formData.email" />
                    <FormKit type="text" label="Teléfono" name="telefono" placeholder="Teléfono XXX-XXX-XXXX"
                        validation="*matches:/^[0-9]{3}-[0-9]{3}-[0-9]{4}$/"
                        :validation-messages="{ matches: 'El formato no es válido' }" v-model="formData.telefono" />
                    <FormKit type="text" label="Empresa" name="empresa" placeholder="Empresa de cliente"
                        v-model="formData.empresa" />
                    <FormKit type="text" label="Puesto" name="puesto" placeholder="Puesto de cliente"
                        v-model="formData.puesto" />
                </FormKit>
            </div>
        </div>
    </div>
</template>

<script setup>
import { onMounted, ref } from "vue"
import ClienteService from "../services/ClienteService.js"
import RouterLink from '../components/UI/RouterLink.vue'
import Heading from '../components/UI/Heading.vue'
import { FormKit } from '@formkit/vue'
import { useRouter, useRoute } from 'vue-router'
const router = useRouter()
const route = useRoute()
const { id } = route.params
const formData = ref({})

onMounted(() => {
    ClienteService.obtenerCliente(id)
        .then(({ data }) => {
            formData.value = data
        })
        .catch((error) => console.error(error))
})
defineProps({
    titulo: {
        type: String,
    },
})
const handleSubmit = (data) => {
    ClienteService.actualizaCliente(id, data)
        .then(() => router.push({ name: 'listado-clientes' }))
        .catch((error) => console.error(error))
}

</script>

<style>
.formkit-wrapper {
    max-width: 100%;
}
</style>