<!-- eslint-disable vue/valid-v-slot -->
<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { ref, computed, onBeforeMount } from 'vue';
import { useDisplay } from 'vuetify';
import { associadosData } from '@/utils/helpers/associados-data';
import BaseBreadcrumb from '@/components/shared/BaseBreadcrumb.vue';
import { useRouter } from 'vue-router';
import UiParentCard from '@/components/shared/UiParentCard.vue';

const { smAndDown } = useDisplay();
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
  return associadosData.find(associado => associado.id === id);
}

const id = Number(router.currentRoute.value.params.id);

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
    title: associadoData.value?.Nome || 'Detalhes',
    disabled: false,
    href: `/dashboard/associados/${id}`
  },
]);
</script>

<template>
  <BaseBreadcrumb :title="page.title" :breadcrumbs="breadcrumbs"></BaseBreadcrumb>

  <v-row>
    <v-col cols="12" md="12">
      <UiParentCard title="Detalhes do Associado">
        <div v-if="associadoData">
          <p><strong>Nome:</strong> {{ associadoData.Nome }}</p>
          <p><strong>Data de Nascimento:</strong> {{ associadoData['Data de Nascimento'] }}</p>
          <p><strong>CPF:</strong> {{ associadoData.CPF }}</p>
          <p><strong>CRM:</strong> {{ associadoData.CRM }}</p>
          <p><strong>Situação de Pagamento:</strong> {{ associadoData['Situação Pagamento'] || 'Não disponível' }}</p>
        </div>
        <div v-else>
          <p>Carregando detalhes do associado...</p>
        </div>
      </UiParentCard>
    </v-col>
  </v-row>
</template>
