<template>
  <q-page style="background-color: #dedcd2" class="column flex">
    <q-card>
      <q-card-section class="flex column q-pb-sm flex-center">
          <span style="text-align: center; color: #57b6ab; font-size: 32px; font-variant: small-caps; font-weight: bold">Denúnciar</span>
          <span style="color: #3e444f">Registrar denúncia.</span>
      </q-card-section>
      <q-card-section class="q-pt-md">
        <q-form
          @submit="onSubmit"
          class="q-gutter-md column"
        >
          <q-input
            filled
            v-model="nomeDenunciante"
            label="Nome do Denunciante"
            hint="Nome que ficará visivel na denúncia."
            lazy-rules
            rounded
            color="teal"
             :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
          />

          <q-input
            filled
            v-model="motivoDenuncia"
            label="Motivo da Denúncia"
            hint="Ex: Negligência"
            lazy-rules
            rounded
            color="teal"
            :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
          />

          <q-input
            filled
            type="number"
            v-model="idadeMenor"
            rounded
            color="teal"
            hint="Informe a possivel idade do menor"
            label="Idade do Menor"
          />
          <q-input
            filled
            v-model="endereco"
            label="Endereço"
            hint="Ex: Rua Tintim Bombado n 24"
            lazy-rules
            rounded
            color="teal"
             :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
          />
          <q-input
            filled
            v-model="descricao"
            label="Descrição"
            lazy-rules
            type="textarea"
            rounded
            color="teal"
            :rules="[ val => val && val.length > 0 || 'Campo Obrigatorio']"
          />
          <q-btn label="Denúnciar" rounded type="submit" color="teal-14"/>
        </q-form>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import {defineComponent, ref} from 'vue'
import {useQuasar} from "quasar";

export default defineComponent({
  name: 'Denunciar',
  methods:{
    onSubmit () {
      if(localStorage.getItem("denunciasSalvas")){
        const denunciasDados = JSON.parse(localStorage.getItem("denunciasSalvas"));
        denunciasDados.push({nomeDenunciante: this.nomeDenunciante, motivoDenuncia: this.motivoDenuncia, idadeMenor: this.idadeMenor, endereco: this.endereco, descricao: this.descricao, data: `${this.dataAtual.getDate()}/${this.dataAtual.getMonth() + 1}/${this.dataAtual.getFullYear()}`})
        localStorage.setItem("denunciasSalvas", JSON.stringify(denunciasDados));
      }else{
        const denunciasDados = [{nomeDenunciante: this.nomeDenunciante, motivoDenuncia: this.motivoDenuncia, idadeMenor: this.idadeMenor, endereco: this.endereco, descricao: this.descricao, data: `${this.dataAtual.getDate()}/${this.dataAtual.getMonth() + 1}/${this.dataAtual.getFullYear()}`}]
        localStorage.setItem("denunciasSalvas", JSON.stringify(denunciasDados));
      }
      this.$q.notify({
        message: 'Denúncia Registrada com sucesso !',
        position: 'top',
        icon: 'done',
        timeout: 2500,
        color: 'positive',
        textColor: 'white',
      })
    }
  },
  setup () {
    const $q = useQuasar()
    const nomeDenunciante = ref(null)
    const motivoDenuncia = ref(null)
    const idadeMenor = ref(null)
    const endereco = ref(null)
    const descricao = ref(null)
    const dataAtual = new Date()
    return {
      nomeDenunciante,
      motivoDenuncia,
      idadeMenor,
      endereco,
      descricao,
      dataAtual,
      $q,
    }
  }
})
</script>

<style scoped>
.cardDenuncia{
  border-radius: 12px;
  background-color: #f1f5f4;
}
.text3lines {
  overflow: hidden;
  display: -webkit-box;
  -webkit-line-clamp: 2; /* number of lines to show */
  line-clamp: 2;
  -webkit-box-orient: vertical;
}
</style>
