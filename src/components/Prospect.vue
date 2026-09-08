<template>
    <div class="row justify-center q-pa-md">
        <!-- Formulario que valida antes de antes emitir @submit -->
        <q-form @submit="onSubmit" class="q-gutter-md">
            <q-input
                filled
                v-model="form.name"
                label="Nombre del Prospecto*"
                :rules = "[ 
                    val => (val && val.length > 0) || 'El nombre es requerido'
                ]"
            />

            <q-input
                filled
                v-model="form.phone"
                label="Telefono *"
                :rules = "[ 
                    val => (val && val.length > 0) || 'El telefono es requerido',
                    val => (val && val.length >= 10) || 'El telefono debe tener al menos 10 digitos'
                ]""
            />

            <div class="row justify-end">
                <q-btn 
                    label="Guardar"
                    type="submint"
                    color="primary"
                    icon="save"
                    :loading="isLoading"
                />
            </div> 
        </q-form>
    </div>        
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useQuasar } from 'quasar'

// Intancia del plugin de Quasar
const $q = useQuasar()

// Estados de la aplicacion
const isLoading = ref(false)
const form = reactive({
    name: '',
    phone: ''
})

// Funcion de envio
const onSubmit = async () => {
    isLoading.value = true

    try {
        // Peticion al sevidor Laravel
        const response = await fetch('http://127.0.0.1:8000/api/prospects', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
            },
            body: JSON.stringify(form)
        })

        const data = await response.json()

        if (response.status == 201){
            $q.notify({
                type: 'positive',
                message: data.message,
                position: 'top-right'
            })

            // limpia el formulario
        } else if (response.status == 422) {

            const firstError = Object.values(dataerror).flat()[0]

            $q.notify({
                type: 'warnig',
                message: firstError,
                position: 'top-right'
            })
        } else {
            // si el sevidor lanza otros errores HTTP (500,400, etc..)
            throw new Error(data.message || 'Ha ocurrido un error')
        } 
    } catch (error){
            // Captura de exepciones
            $q.notify({
                type: 'negative',
                message: 'Error de solicitud con el servidor',
                position: 'top-right'
            })
    } finally {
        isLoading.value = false
    }
}

</script>