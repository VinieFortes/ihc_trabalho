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
        v-model="email"
        label="Email"
        hint="Ex: usuario@email.com"
        lazy-rules
        rounded
        color="teal"
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
      />

      <q-input
        filled
        type="password"
        v-model="pass"
        rounded
        color="teal"
        label="Senha"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
      />
      <q-btn label="Acessar" rounded type="submit" color="teal-14"/>
      <q-toggle v-model="accept" color="teal-14" label="Fazer Login como Administrador." />

    </q-form>
    <q-space/>
    <q-separator spaced/>
    <div class="flex flex-center">
      <span>
        Não tenho uma conta. <span style="color: #57b6ab" @click="$router.push('/signup')">Toque para criar uma agora.</span>
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
    onSubmit(){
      if(localStorage.getItem("userDados")){
        const dados = JSON.parse(localStorage.getItem("userDados"));
        if(dados.email === this.email && dados.pass === this.pass){
          if(this.accept){
            localStorage.setItem('isLogged', 'true')
            const dadosUser = JSON.parse(localStorage.getItem("userDados"));
            localStorage.setItem("userDados", JSON.stringify({nome: dadosUser.nome, email: dadosUser.email, pass: dadosUser.pass, isAdm: true}))
            this.$router.push('/')
            this.$q.notify({
              message: 'Login Realizado com Sucesso !',
              position: 'top',
              icon: 'done',
              timeout: 2500,
              color: 'positive',
              textColor: 'white',
            })
          }else{
            localStorage.setItem('isLogged', 'true')
            const dadosUser = JSON.parse(localStorage.getItem("userDados"));
            localStorage.setItem("userDados", JSON.stringify({nome: dadosUser.nome, email: dadosUser.email, pass: dadosUser.pass, isAdm: false}))
            this.$router.push('/')
            this.$q.notify({
              message: 'Login Realizado com Sucesso !',
              position: 'top',
              icon: 'done',
              timeout: 2500,
              color: 'positive',
              textColor: 'white',
            })
          }
        }else{
          this.$q.notify({
            message: 'Email ou Senha Incorreto',
            position: 'top',
            icon: 'close',
            timeout: 2500,
            color: 'negative',
            textColor: 'white',
          })
        }
      }else{
        this.$q.notify({
          message: 'Email ou Senha Incorreto',
          position: 'top',
          timeout: 2500,
          icon: 'close',
          color: 'negative',
          textColor: 'white',
        })
      }
    }
  },
  setup () {
    const $q = useQuasar()

    const email = ref(null)
    const pass = ref(null)
    const accept = ref(false)

    return {
      email,
      pass,
      $q,
      accept,
    }
  }
})
</script>
