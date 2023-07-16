<script setup>
import {onMounted, reactive, ref, watch} from 'vue'
  import { uid } from 'uid'
  import Header from "./components/Header.vue";
  import Form from "./components/Form.vue";
  import Patient from "./components/Patient.vue";

  const pacientes = ref([]) // este es para el listado de pacientes

  const paciente = reactive({
      id: null,
      name: '',
      owner: '',
      email: '',
      uploaded_to: '',
      symptoms: ''
  })

  onMounted(() => {
      const pacientesStorage = localStorage.getItem('pacientes')
      if ( pacientesStorage ) {
          pacientes.value = JSON.parse(pacientesStorage)
      }
  })

  watch(pacientes, () => {
      guardarStorage()
  }, {
      deep: true
  })

  const guardarStorage = () => {
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }

  const guardarPaciente = () => {

      if ( paciente.id ) {
          const { id } = paciente
          const i = pacientes.value.findIndex((patient) => patient.id === id)
          pacientes.value[i] = { ...paciente }
      } else {
          pacientes.value.push({
              ...paciente,
              id: uid()
          })
      }

      // reiniciar valores del formulario.
      Object.assign(paciente, {
          name: '',
          owner: '',
          email: '',
          uploaded_to: '',
          symptoms: '',
          id: null
      })

  }

  const updatePatient = id => {
      const editPatient = pacientes.value.filter( patient => patient.id === id )[0]

      Object.assign(paciente, editPatient)
  }

  const deletePatient = id => {
      pacientes.value = pacientes.value.filter( paciente => paciente.id !== id )
  }


</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

      <div class="mt-12 md:flex">
          <Form
                  v-model:name="paciente.name"
                  v-model:owner="paciente.owner"
                  v-model:email="paciente.email"
                  v-model:uploaded_to="paciente.uploaded_to"
                  v-model:symptoms="paciente.symptoms"
                  @guardar-paciente="guardarPaciente"
                  :id="paciente.id"
          />

          <div class="md:w-1/2 md:h-screen overflow-y-scroll">
              <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
              <p class="text-lg mt-5 mb-10 text-center">
                  Información de
                  <span class="text-indigo-600 font-bold">pacientes</span>
              </p>

              <div v-if="pacientes.length > 0">
                <Patient
                        v-for="paciente in pacientes"
                        :paciente="paciente"
                        @update-patient="updatePatient"
                        @delete-patient="deletePatient"
                />
              </div>
              <p class="mt-10 text-2xl text-center" v-else>No hay pacientes</p>

          </div>

      </div>
  </div>
</template>
