<template>
    <div class="row justify-center q-pa-md">
        <!-- Formulario que valida antes de antes emitir @submit -->
        <q-form @submit="onSubmit" class="q-gutter-md">
            <q-input
                filled
                v-model="prospect.name"
                label="Nombre del Prospecto*"
                :rules = [ 
                    val => (val && val.length > 0) || 'El nombre es requerido'
                ]
            />

            <q-input
                filled
                v-model="from.phone"
                label="Telefono *"
                :rules = [ 
                    val => (val && val.length > 0) || 'El telefono es requerido'
                    val => (val && val.length >= 10) || 'El telefono debe tener al menos 10 digitos'
                ]
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