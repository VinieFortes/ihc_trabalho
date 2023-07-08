<template>
  <q-page class="q-pa-md column flex">
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
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
      />

      <q-input
        filled
        v-model="email"
        label="Email"
        lazy-rules
        rounded
        color="teal"
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
      />

      <q-input
        filled
        type="tel"
        v-model="telefone"
        label="Telefone"
        lazy-rules
        rounded
        color="teal"
        :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
      />

      <q-btn label="Alterar Dados" rounded type="submit" color="teal-14"/>
    </q-form>
    <q-space/>
    <q-separator spaced/>
    <div class="flex">
      <q-btn @click="fazerLogOut" style="flex: 1" label="Sair" rounded color="red"/>
    </div>
  </q-page>
</template>

<script>
import {defineComponent, ref} from 'vue'
import {useQuasar} from "quasar";

export default defineComponent({
  name: 'Perfil',

  methods:{
    fazerLogOut(){
      localStorage.removeItem('isLogged')
      this.$router.push('/login')
      this.$q.notify({
        message: 'Log-out feito com sucesso!',
        position: 'top',
        icon: 'done',
        timeout: 2500,
        color: 'positive',
        textColor: 'white',
      })
    },
    onSubmit () {
      const dados = JSON.parse(localStorage.getItem("userDados"));
      localStorage.setItem("userDados", JSON.stringify({nome: this.nome, email: this.email, telefone: this.telefone, pass: dados.pass}))
      this.$q.notify({
        message: 'Dados atualizados com sucesso!',
        position: 'top',
        icon: 'done',
        timeout: 2500,
        color: 'positive',
        textColor: 'white',
      })
    },
  },
  mounted() {
    if(localStorage.getItem("userDados")){
      const dados = JSON.parse(localStorage.getItem("userDados"));
      this.nome = dados.nome;
      this.email = dados.email;
      if(dados.telefone){
        this.telefone = dados.telefone;
      }
    }
  },
  setup () {
    const $q = useQuasar()
    const nome = ref(null)
    const email = ref(null)
    const telefone = ref(null)

    return {
      nome,
      email,
      $q,
      telefone,
    }
  }
})
</script>

<style scoped>
</style>
