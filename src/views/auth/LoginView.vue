<script setup>
    import { inject } from 'vue'
    import { useRouter } from 'vue-router'
    import AuthAPI from '../../api/AuthAPI'
    
    const toast = inject('toast')
    const router = useRouter()

    const handleSubmit = async(formData) => {
        try {
            const { data: {token} } = await AuthAPI.login(formData)
            localStorage.setItem('AUTH_TOKEN', token)
            router.push({name: 'my-appointments'})
        } catch (error) {
            toast.open({
                message: error.response.data.msg,
                type: 'error'
            })
        }
    }
</script>

<template>
    <h1 class="text-6xl font-extrabold text-white text-center mt-10">Iniciar Sesión</h1>
    <p class="text-2xl text-white text-center my-5">Si tienes una cuenta, inicia sesión</p>

    <FormKit
        id="loginForm"
        type="form"
        :actions="false"
        incomplete-message="No se pude enviar, revisa los mensajes"
        @submit="handleSubmit"
    >

        <FormKit
            type="email"
            label="Correo electrónico"
            name="email"
            placeholder="Correo de Usuario"
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
            placeholder="Contraseña de Usuario"
            validation="required"
            :validation-messages="{
                required: 'La Contraseña es obligatoria'
            }"
        />

        <FormKit type="submit">Iniciar Sesión</FormKit>
    </FormKit>
</template>
