<template>
  <q-page style="background-color: #dedcd2" class="column flex">
    <q-card v-if="minhasDenuncias.length === 0">
      <q-card-section class="flex flex-center">
        <span style="color: #57b6ab; font-size: 24px; font-variant: small-caps; font-weight: bold">
          Nenhuma Denúncia Registrada.
        </span>
      </q-card-section>
    </q-card>
    <q-card v-if="minhasDenuncias.length > 0" >
      <q-card-section class="flex row q-pa-sm">
        <q-btn v-if="showMinhasDenuncias" @click="showMinhasDenuncias = false;" flat color="teal" icon="expand_less"/>
        <q-btn v-else @click="showMinhasDenuncias = true;" flat color="teal" icon="expand_more"/>
        <span style="color: #57b6ab; font-size: 24px; font-variant: small-caps; font-weight: bold">
          Minhas Denúncias
          <q-chip dense color="teal" text-color="white">{{ minhasDenuncias.length }}</q-chip>
        </span>
      </q-card-section>
      <q-card-section v-if="showMinhasDenuncias" class="q-pa-sm q-gutter-y-sm">
        <q-card v-for="(denuncia, index) of minhasDenuncias" class="cardDenuncia flex row flex-center" bordered>
          <div style="flex: 1" class="flex column">
            <q-card-section class="q-pb-none">
              <span style="font-size: 18px; font-weight: bold">{{ denuncia.motivoDenuncia }} - <span style="font-weight: normal; font-size: 16px">{{ denuncia.data }}</span></span>
            </q-card-section>
            <q-card-section class="q-pb-none">
              <div class="text3lines">{{ denuncia.descricao }}</div>
            </q-card-section>
            <q-card-section>
              {{ denuncia.endereco }}
            </q-card-section>
          </div>
          <q-btn icon="more_vert" size="md" flat>
            <q-menu
              transition-show="scale"
              transition-hide="scale">
              <q-list dense style="min-width: 100px">
                <q-item  clickable v-close-popup>
                  <q-item-section @click="editarDenunciaFunc(index)">Editar</q-item-section>
                </q-item>
                <q-item clickable v-close-popup>
                  <q-item-section @click="deleteDenuncia(index)">Deletar</q-item-section>
                </q-item>
              </q-list>
            </q-menu>
          </q-btn>
        </q-card>
      </q-card-section>
    </q-card>
    <q-card v-if="isAdm" class="q-mt-sm">
      <q-card-section class="flex q-pa-sm">
        <q-btn v-if="showDenunciasRecentes" @click="showDenunciasRecentes = false;" flat color="teal" icon="expand_less"/>
        <q-btn v-else @click="showDenunciasRecentes = true;" flat color="teal" icon="expand_more"/>
        <span style="text-align: center; color: #57b6ab; font-size: 24px; font-variant: small-caps; font-weight: bold">
          Denúncias Recentes
          <q-chip dense color="teal" text-color="white">{{denunciasRecentes.length}}
          </q-chip>
        </span>
      </q-card-section>
      <q-card-section v-if="showDenunciasRecentes" class="q-pa-sm q-gutter-y-sm" >
        <q-card v-for="denuncia of denunciasRecentes" class="cardDenuncia flex row flex-center" bordered>
          <div style="flex: 1" class="flex column">
            <q-card-section class="q-pb-none">
              <span style="font-size: 18px; font-weight: bold">{{ denuncia.motivoDenuncia }} - <span style="font-weight: normal; font-size: 16px">{{ denuncia.data }}</span></span>
            </q-card-section>
            <q-card-section class="q-pb-none">
              <div class="text3lines">{{ denuncia.descricao }}</div>
            </q-card-section>
            <q-card-section>
              {{ denuncia.endereco }}
            </q-card-section>
          </div>
        </q-card>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import {defineComponent, ref} from 'vue'

export default defineComponent({
  name: 'Home',
  methods:{
    deleteDenuncia(idx){
      this.minhasDenuncias.splice(idx, 1);
      localStorage.setItem("denunciasSalvas", JSON.stringify(this.minhasDenuncias))
      this.$q.notify({
        message: 'Denúncia Deletada com sucesso !',
        position: 'top',
        icon: 'done',
        timeout: 2500,
        color: 'positive',
        textColor: 'white',
      })
    },
    editarDenunciaFunc(idx){
      this.$emit("editDenuncia", idx)
    }
  },
  mounted() {
    if(localStorage.getItem("denunciasSalvas")){
      this.minhasDenuncias = JSON.parse(localStorage.getItem("denunciasSalvas"));
    }
    const dadosUser =  JSON.parse(localStorage.getItem("userDados"));
    if(dadosUser.isAdm === true){
      console.log('here')
      this.isAdm = true;
    }
  },
  setup(){
    const minhasDenuncias = ref([])
    const isAdm = ref(false)
    const denunciasRecentes = [
      {
        nomeDenunciante: 'Jose',
        motivoDenuncia: 'Negligencia',
        idadeMenor: '10',
        endereco: 'Rua dos Bandeirantes n 50, São Paulo - SP',
        descricao: 'Menor visto em estado de negligencia em situação de rua',
        data: '02/06/2023'
      },
      {
        nomeDenunciante: 'Maria',
        motivoDenuncia: 'Violência doméstica',
        idadeMenor: '7',
        endereco: 'Avenida das Flores n 100, Rio de Janeiro - RJ',
        descricao: 'Criança exposta a agressões físicas constantes no ambiente familiar',
        data: '10/07/2023'
      },
      {
        nomeDenunciante: 'Ana',
        motivoDenuncia: 'Trabalho infantil',
        idadeMenor: '12',
        endereco: 'Rua das Palmeiras n 80, Belo Horizonte - MG',
        descricao: 'Menor trabalhando em condições inadequadas em uma oficina mecânica',
        data: '18/07/2023'
      },
      {
        nomeDenunciante: 'Pedro',
        motivoDenuncia: 'Abandono',
        idadeMenor: '3',
        endereco: 'Rua dos Girassóis n 30, Salvador - BA',
        descricao: 'Criança encontrada sozinha em casa, em estado de abandono',
        data: '25/07/2023'
      },
      {
        nomeDenunciante: 'Lúcia',
        motivoDenuncia: 'Abuso sexual',
        idadeMenor: '14',
        endereco: 'Avenida dos Sonhos n 70, Fortaleza - CE',
        descricao: 'Menor vítima de abuso sexual por parte de um familiar',
        data: '03/08/2023'
      },
      {
        nomeDenunciante: 'Rafael',
        motivoDenuncia: 'Fome extrema',
        idadeMenor: '5',
        endereco: 'Rua das Oliveiras n 20, Recife - PE',
        descricao: 'Criança vivendo em condições de extrema pobreza, sem acesso a alimentos adequados',
        data: '11/08/2023'
      },
      {
        nomeDenunciante: 'Carolina',
        motivoDenuncia: 'Exploração de trabalho infantil',
        idadeMenor: '11',
        endereco: 'Avenida dos Coqueiros n 60, Florianópolis - SC',
        descricao: 'Menor trabalhando em uma plantação de tomates por longas horas',
        data: '19/08/2023'
      },
      {
        nomeDenunciante: 'Fernando',
        motivoDenuncia: 'Maus-tratos',
        idadeMenor: '8',
        endereco: 'Rua das Acácias n 40, Brasília - DF',
        descricao: 'Criança sofrendo maus-tratos físicos e psicológicos por parte dos pais',
        data: '27/08/2023'
      },
      {
        nomeDenunciante: 'Isabela',
        motivoDenuncia: 'Tráfico de drogas',
        idadeMenor: '16',
        endereco: 'Avenida das Orquídeas n 90, Curitiba - PR',
        descricao: 'Menor envolvido com tráfico de drogas em uma região de alta criminalidade',
        data: '04/09/2023'
      },
      {
        nomeDenunciante: 'Ricardo',
        motivoDenuncia: 'Exploração sexual',
        idadeMenor: '13',
        endereco: 'Rua das Rosas n 10, Porto Alegre - RS',
        descricao: 'Menor vítima de exploração sexual em um prostíbulo clandestino',
        data: '12/09/2023'
      }
    ];
    const showMinhasDenuncias = ref(true)
    const showDenunciasRecentes = ref(true)
    const showMenuDenuncia = ref(false)
    return{
      minhasDenuncias: minhasDenuncias,
      denunciasRecentes: denunciasRecentes,
      showMinhasDenuncias: showMinhasDenuncias,
      showDenunciasRecentes: showDenunciasRecentes,
      showMenuDenuncia: showMenuDenuncia,
      isAdm: isAdm
    }
  },
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
