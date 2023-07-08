<template>
  <q-page class="q-pa-md column flex">
    <img
      alt="Quasar logo"
      src="iconCiano.png"
      class="q-ma-sm"
      style="width: 92px; height: 98px; align-self: center"
    >
    <q-form
      @submit="onSubmit"
      class="q-mt-xl q-gutter-md column"
    >
      <q-input
        filled
        v-model="nome"
        label="Nome"
        lazy-rules
        rounded
        color="teal"
        :rules="[ val => val && val.length > 0 || 'Campo obrigatorio']"
      />

      <q-input
        filled
        v-model="email"
        label="Email"
        lazy-rules
        rounded
        color="teal"
        :rules="[ val => val && val.length > 0 || 'Campo obrigatorio']"
      />

      <q-input
        filled
        type="password"
        v-model="pass"
        rounded
        color="teal"
        label="Senha"
        lazy-rules
        :rules="[
          val => val.length <= 8 || 'Coloque uma senha valida com no minimo 8 caracteres',
        ]"
      />
      <q-btn label="Registrar" rounded type="submit" color="teal-14"/>

    </q-form>
    <q-space/>
    <q-separator spaced/>
    <div class="flex flex-center">
      <span>
        Já possui uma conta ? <span style="color: #57b6ab" @click="$router.push('/login')">Toque aqui para acessar.</span>
      </span>
    </div>
  </q-page>
</template>

<script>
import {defineComponent, ref} from 'vue'
import {useQuasar} from "quasar";

export default defineComponent({
  name: 'Login',

  methods:{
    onSubmit (){
      localStorage.setItem("userDados", JSON.stringify({nome: this.nome, email: this.email, pass: this.pass}))
      localStorage.setItem("isLogged", 'true')
      this.$router.push('/')
    }
  },
  setup () {
    const $q = useQuasar()

    const nome = ref(null)
    const email = ref(null)
    const pass = ref(null)
    const accept = ref(false)

    return {
      nome,
      email,
      pass,
      accept,

    }
  }
})
</script>
