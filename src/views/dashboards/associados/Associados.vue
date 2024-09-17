<!-- eslint-disable vue/valid-v-slot -->
<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { ref, computed } from 'vue';
import { useDisplay } from 'vuetify';
import { associadosData } from '@/utils/helpers/associados-data';
import BaseBreadcrumb from '@/components/shared/BaseBreadcrumb.vue';
import { useRouter } from 'vue-router';

const { smAndDown } = useDisplay();
const router = useRouter()

const items = ref(
  associadosData.map(associado => ({
    ...associado,
    'Data de Nascimento': new Date(associado['Data de Nascimento']).toLocaleDateString('pt-BR'),
  }))
);

const page = ref({ title: 'Associados' });
const breadcrumbs = ref([
  {
    title: 'Associados',
    disabled: true,
    href: '#'
  },
]);

const isMobile = computed(() => smAndDown.value);

const itemsPerPage = 5;
const currentPage = ref(1);

const paginatedItems = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage;
  const end = start + itemsPerPage;
  return items.value.slice(start, end);
});

const totalPages = computed(() => Math.ceil(items.value.length / itemsPerPage));

</script>

<template>
  <BaseBreadcrumb :title="page.title" :breadcrumbs="breadcrumbs"></BaseBreadcrumb>

    <v-data-table
        v-if="!isMobile"
        class="rounded text"
        :items="paginatedItems"
        :headers="[
            { title: 'Nome', key: 'Nome' },
            { title: 'Data de Nascimento', key: 'Data de Nascimento' },
            { title: 'CPF', key: 'CPF' },
            { title: 'CRM', key: 'CRM' },
            { title: 'Ação', key: 'actions', sortable: false }
        ]"
        :hide-default-footer="true"
    >
      <template #item.dataDeNascimento="{ item }">
          {{ item['Data de Nascimento'] }}
      </template>
      <template #item.actions="{ item }">
        <router-link :to="{ name: 'AssociadoDetail', params: { id: item.id } }" style="text-decoration: none; color: inherit;">
          <EyeIcon class="icon" />
        </router-link>
      </template>
    </v-data-table>

    <v-container v-else>
      <v-row>
        <v-col
          v-for="(item, index) in paginatedItems"
          :key="index"
          cols="12"
          sm="6"
          md="4"
        >
          <router-link :to="`/dashboard/associados/${item.id}`" class="v-link" style="text-decoration: none; color: inherit;">
            <v-card class="mb-4 rounded clickable-card">
              <v-card-title>{{ item.Nome }}</v-card-title>
              <v-card-subtitle>CPF: {{ item.CPF }}</v-card-subtitle>
              <v-card-text>
                <p>Data de Nascimento: {{ item['Data de Nascimento'] }}</p>
                <p>CRM: {{ item.CRM }}</p>
              </v-card-text>
            </v-card>
          </router-link>
        </v-col>
      </v-row>
    </v-container>

    <v-pagination
      v-if="!isMobile"
      v-model:page="currentPage"
      :length="totalPages"
      class="mt-4"
    ></v-pagination>

    <v-pagination
      v-if="isMobile"
      v-model:page="currentPage"
      :length="totalPages"
      class="mt-4"
    ></v-pagination>
</template>
