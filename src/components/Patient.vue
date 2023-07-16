<script setup>

    defineEmits(['update-patient', 'delete-patient'])

    defineProps({
        paciente: {
            type: Object,
            required: true
        }
    })
</script>

<template>
    <div class="mx-5 my-10 bg-white shadow-md px-5 py-10 rounded-xl">
        <p class="font-bold mb-3 text-gray-700 uppercase">ID:
            <span class="font-normal normal-case">
                {{ paciente.id }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Nombre:
            <span class="font-normal normal-case">
              {{ paciente.name }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Propietario:
            <span class="font-normal normal-case">
              {{ paciente.owner }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Email:
            <span class="font-normal normal-case">
              {{ paciente.email }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Fecha Alta:
            <span class="font-normal normal-case">
              {{ paciente.uploaded_to }}
            </span>
        </p>

        <p class="font-bold mb-3 text-gray-700 uppercase">Síntomas:
            <span class="font-normal normal-case">
              {{ paciente.symptoms }}
            </span>
        </p>

        <div class="grid md:grid-cols-2  gap-5 mt-10 ">
            <button
                    type="button"
                    class="block w-full py-2 px-10 bg-indigo-600 hover:bg-indigo-700 text-white font-bold uppercase rounded-lg"
                    @click="$emit('update-patient', paciente.id)"
            >Editar</button>

            <button
                    type="button"
                    class="block w-full py-2 px-10 bg-red-600 hover:bg-red-700 text-white font-bold uppercase rounded-lg"
                    @click="$emit('delete-patient', paciente.id)"
            >Eliminar</button>
        </div>
    </div>
</template>
