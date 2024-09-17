<!-- eslint-disable vue/valid-v-slot -->
<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { ref, computed, onBeforeMount } from 'vue';
import { useDisplay } from 'vuetify';
import { associadosData } from '@/utils/helpers/associados-data';
import BaseBreadcrumb from '@/components/shared/BaseBreadcrumb.vue';
import { useRoute, useRouter } from 'vue-router';
import UiParentCard from '@/components/shared/UiParentCard.vue';

const { smAndDown } = useDisplay();
const route = useRoute();
const router = useRouter();

interface Associado {
  id: number;
  Nome: string;
  "Data de Nascimento": string;
  CPF: string;
  CRM: string;
  "Situação Pagamento"?: string;
}

function getAssociado(id: number): Associado | undefined {
  const associado = associadosData.find(associado => associado.id === id);
  if (associado) {
    associado['Data de Nascimento'] = new Date(associado['Data de Nascimento']).toLocaleDateString('pt-BR');;
  }
  return associado;
}

const id = Number(route.params.id);

const associadoData = ref<Associado | undefined>(undefined);

onBeforeMount(() => {
  associadoData.value = getAssociado(id);
});

const page = ref({ title: 'Detalhes do Associado' });
const breadcrumbs = computed(() => [
  {
    title: 'Associados',
    disabled: false,
    href: '/dashboard/associados'
  },
  {
    title: associadoData.value?.nome || 'Detalhes',
    disabled: true,
    href: `/dashboard/associados/${id}`
  },
]);

const isMobile = computed(() => smAndDown.value);

const saveChanges = () => {
  // Implement save logic here
  console.log('Changes saved:', associadoData.value);
  // Optionally navigate back to the list
  router.push('/dashboard/associados');
};
</script>

<template>
  <BaseBreadcrumb :title="page.title" :breadcrumbs="breadcrumbs"></BaseBreadcrumb>

  <v-row>
    <v-col cols="12" md="12">
      <UiParentCard :title="associadoData?.nome || 'Detalhes do Associado'">
        <div v-if="associadoData">
          <v-form>
            <v-row>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="associadoData.nome"
                  label="Nome"
                  variant="outlined"
                  required
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="associadoData.data_de_nascimento"
                  label="Data de Nascimento"
                  variant="outlined"
                  mask="##/##/####" 
                  required
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="associadoData.cpf"
                  label="CPF"
                  variant="outlined"
                  required
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="associadoData.crm"
                  label="CRM"
                  variant="outlined"
                  required
                />
              </v-col>
              <v-col cols="12" sm="4">
                <v-text-field
                  v-model="associadoData.situacao_pagamento"
                  label="Situação de Pagamento"
                  variant="outlined"
                />
              </v-col>
            </v-row>
          </v-form>
        </div>
        <div v-else>
          <p>Carregando detalhes do associado...</p>
        </div>
        <v-btn
          class="mt-4"
          color="primary"
          @click="saveChanges"
        >
          Salvar
        </v-btn>
      </UiParentCard>
    </v-col>
  </v-row>
</template>
