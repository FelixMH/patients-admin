<script setup>
import {computed, reactive} from 'vue'
    import Alert from "./Alert.vue";

    const stateAlerta = reactive({
        type: '',
        message: ''
    })

    const emitMethods = defineEmits([
        'update:name',
        'update:owner',
        'update:email',
        'update:uploaded_to',
        'update:symptoms',
        'guardar-paciente'])

    const props = defineProps({
        id: {
            type: [String, null],
            required: true
        },
        name: {
            type: String,
            required: true
        },
        owner: {
            type: String,
            required: true
        },
        email: {
            type: String,
            required: true
        },
        uploaded_to: {
            type: String,
            required: true
        },
        symptoms: {
            type: String,
            required: true
        }
    })

    const editando = computed(() => {
        return props.id
    })

    const validar = e => {

        if ( Object.values(props).includes('') ) {
            stateAlerta.message = 'Todos los campos son obligatorios.'
            stateAlerta.type = 'error'

            setTimeout(() => {
                Object.assign(stateAlerta, {
                    type: '',
                    message: ''
                })
            }, 3000)

            return
        }

        emitMethods('guardar-paciente')
        stateAlerta.message = 'Paciente agregado correctamente.'
        stateAlerta.type = 'exito'

        setTimeout(() => {
            Object.assign(stateAlerta, {
                type: '',
                message: ''
            })
        }, 3000)


    }

</script>

<template>
  <div class="md:w-1/2">
      <h2 class="font-black text-3xl text-center">Seguimiento pacientes</h2>

      <p class="text-lg mt-5 mb-10 text-center">
          Añade pacientes y
          <span class="text-indigo-600 font-bold">admínistralos</span>
      </p>

      <Alert
          v-if="stateAlerta.message"
          :alerta="stateAlerta"
      />

      <form
           class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
           @submit.prevent="validar">
          <div class="mb-5">
              <label
                      for="mascota"
                      class="block text-gray-700 uppercase font-bold cursor-pointer"
              >
                  Añade mascota
              </label>

              <input
                      id="mascota"
                      type="text"
                      placeholder="nombre de la mascota"
                      class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                      @input="$emit('update:name', $event.target.value)"
                      :value="name"

              />
          </div>

        <!--  campo propietario / dueño        -->
          <div class="mb-5">
              <label
                      for="dueño"
                      class="block text-gray-700 uppercase font-bold cursor-pointer"
              >
                  Añade dueño
              </label>

              <input
                      id="dueño"
                      type="text"
                      placeholder="nombre del dueño"
                      class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                      @input="$emit('update:owner', $event.target.value)"
                      :value="owner"
              />
          </div>

        <!-- Campo email         -->
          <div class="mb-5">
              <label
                      for="email"
                      class="block text-gray-700 uppercase font-bold cursor-pointer"
              >
                  Añade email
              </label>

              <input
                      id="email"
                      type="email"
                      placeholder="email"
                      class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                      @input="$emit('update:email', $event.target.value)"
                      :value="email"

              />
          </div>

        <!-- Campo fecha de alta         -->
          <div class="mb-5">
              <label
                      for="alta"
                      class="block text-gray-700 uppercase font-bold cursor-pointer"
              >
                  alta
              </label>

              <input
                      id="alta"
                      type="date"
                      class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
                      @input="$emit('update:uploaded_to', $event.target.value)"
                      :value="uploaded_to"
              />
          </div>

        <!-- Campo síntomas         -->
          <div class="mb-5">
              <label
                      for="sintomas"
                      class="block text-gray-700 uppercase font-bold cursor-pointer"
              >
                  Síntomas
              </label>

              <textarea
                      id="sintomas"
                      type="text"
                      placeholder="Síntomas"
                      class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-40"
                      @input="$emit('update:symptoms', $event.target.value)"
                      :value="symptoms"

              />
          </div>

          <input
                  type="submit"
                  class="bg-indigo-600 w-full p-3 text-white uppercase font-bold hover:bg-indigo-700
                  cursor-pointer transition-colors"
                  :value="[ editando ? 'Guardar cambios' : 'Registrar paciente']"
          >

      </form>
  </div>
</template>