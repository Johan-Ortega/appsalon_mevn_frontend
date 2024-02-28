<script setup>
    import { inject } from 'vue'
    import { reset } from '@formkit/vue'
    import AuthAPI from '../../api/AuthAPI'

    const toast = inject('toast')

    const handleSubmit = async ({password_confirm, ...formData}) => {
        try {
            const { data } = await AuthAPI.register(formData)
            toast.open({
                message: data.msg,
                type: 'success'
            })
            reset('registerForm')
        } catch (error) {
            toast.open({
                message: error.response.data.msg,
                type: 'error'
            })
        }
    }

</script>

<template>
    <h1 class="text-6xl font-extrabold text-white text-center mt-10">Crea una cuenta</h1>
    <p class="text-2xl text-white text-center my-5">Crea una cuenta en AppSalon</p>

    <FormKit
        id="registerForm"
        type="form"
        :actions="false"
        incomplete-message="No se pude enviar, revisa los mensajes"
        @submit="handleSubmit"
    >
        <FormKit
            type="text"
            label="Nombre"
            name="name"
            placeholder="Tu Nombre"
            validation="required|length:3"
            :validation-messages="{
                required: 'El Nombre es obligatorio',
                length: 'El Nombre es muy corto'
            }"
        />

        <FormKit
            type="email"
            label="Correo electrónico"
            name="email"
            placeholder="Correo de Registro"
            validation="required|email"
            :validation-messages="{
                required: 'El Correo es obligatorio',
                email: 'Correo no válido'
            }"
        />

        <FormKit
            type="password"
            label="Contraseña"
            name="password"
            placeholder="Contraseña de Usuario - Min 8 caracteres"
            validation="required|length:8"
            :validation-messages="{
                required: 'La Contraseña es obligatoria',
                length: 'La Contraseña debe contener al menos 8 caracteres'
            }"
        />
        
        <FormKit
            type="password"
            label="Repite tu contraseña"
            name="password_confirm"
            placeholder="Repite la contraseña"
            validation="required|confirm"
            :validation-messages="{
                required: 'La confirmación de contraseña es obligatoria',
                confirm: 'Las contraseñas no son iguales'
            }"
        />

        <FormKit type="submit">Crear Cuenta</FormKit>
    </FormKit>
</template>
