<template>
  <q-page class="column flex">
    <q-header style="background-color: #57b6ab" class="q-pa-sm">
      <div class="flex row justify-between">
        <q-icon v-if="tab !== 'home'" @click="tab = 'home'" size="xl" color="white" name="arrow_back"/>
        <q-space v-else/>
        <q-icon @click="tab = 'perfil'" size="xl" color="white" name="account_circle"/>
      </div>
    </q-header>
    <q-tab-panels v-model="tab" animated class="shadow-2 rounded-borders">
      <q-tab-panel class="no-padding" name="home">
        <Home @editDenuncia="openEditarDenuncia"/>
      </q-tab-panel>

      <q-tab-panel class="no-padding" name="search">
        <Pesquisa/>
      </q-tab-panel>

      <q-tab-panel class="no-padding" name="denunciar">
        <Denunciar/>
      </q-tab-panel>

      <q-tab-panel class="no-padding" name="perfil">
        <Perfil/>
      </q-tab-panel>

      <q-tab-panel class="no-padding" name="denunciarEdit">
        <EditarDenunciar :idx-denuncia="idxEditarDenuncia"/>
      </q-tab-panel>
    </q-tab-panels>
    <q-footer style="background-color: #57b6ab" class="q-pa-sm">
      <q-tabs
        v-model="tab"
        class="text-white flex"
      >
        <q-tab name="search" icon="search" label="Pesquisar" />
        <q-tab name="home" icon="home" label="Home" />
        <q-tab name="denunciar" icon="article" label="Denúnciar" />
      </q-tabs>
    </q-footer>
  </q-page>
</template>

<script>
import {defineComponent, ref} from 'vue'
import Home from "components/Home.vue";
import Denunciar from "components/Denunciar.vue";
import Pesquisa from "components/Pesquisa.vue";
import Perfil from "components/Perfil.vue";
import EditarDenunciar from "components/EditarDenuncia.vue";

export default defineComponent({
  name: 'IndexPage',
  components:{
    EditarDenunciar,
    Perfil,
    Pesquisa,
    Denunciar,
    Home
  },
  methods:{
    openEditarDenuncia(idx){
      this.idxEditarDenuncia = idx;
      this.tab = 'denunciarEdit';
    }
  },
  setup(){
    const idxEditarDenuncia = ref(null);
    return{
      tab: ref('home'),
      idxEditarDenuncia: idxEditarDenuncia
    }
  }
})
</script>
