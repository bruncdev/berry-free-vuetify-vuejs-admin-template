<script setup lang="ts">
import UiParentCard from '@/components/shared/UiParentCard.vue';
import { ref } from 'vue';
import FullLayout from '@/layouts/full/FullLayout.vue';
import VerticalHeader from '@/layouts/full/vertical-header/VerticalHeader.vue';
import VerticalSidebar from '@/layouts/full/vertical-sidebar/VerticalSidebar.vue';

const metodoPagamento = ref('cartao');
</script>

<template>
    <VerticalHeader />
    <VerticalSidebar />
  <v-row justify="center">
    <v-col cols="12" md="6">
      <UiParentCard title="Gerar Pagamento" class="pa-6">
        <v-row class="mb-4">
          <v-col>
            <p class="text-h6 font-weight-bold mb-2">Descrição:</p>
            <p class="text-body-1">Pagamento da Anuidade 2024</p>
          </v-col>
          <v-col class="text-right">
            <p class="text-h6 font-weight-bold mb-2">Valor:</p>
            <p class="text-body-1">R$ 200,00</p>
          </v-col>
        </v-row>

        <v-row class="mb-4">
          <v-col cols="12">
            <v-radio-group
              v-model="metodoPagamento"
              row
              class="mt-4"
              label="Selecione o método de pagamento"
            >
              <v-radio label="Cartão de Crédito" value="cartao"></v-radio>
              <v-radio label="Boleto Bancário" value="boleto"></v-radio>
            </v-radio-group>
          </v-col>
        </v-row>

        <v-row>
          <v-col cols="12" v-if="metodoPagamento === 'cartao'">
            <v-alert type="info" class="mb-4">
              Você escolheu pagar com Cartão de Crédito.
            </v-alert>
            <v-text-field label="Número do Cartão" outlined></v-text-field>
            <v-text-field label="Nome no Cartão" outlined></v-text-field>
            <v-row>
              <v-col cols="6">
                <v-text-field label="Validade" outlined></v-text-field>
              </v-col>
              <v-col cols="6">
                <v-text-field label="CVV" outlined></v-text-field>
              </v-col>
            </v-row>
          </v-col>

          <v-col cols="12" v-else>
            <v-alert type="info" class="mb-4">
              Você escolheu pagar com Boleto Bancário.
            </v-alert>
            <v-btn color="primary" block class="mb-2">
              Gerar Boleto
            </v-btn>
          </v-col>
        </v-row>
        
        <v-btn color="primary" block class="mt-4">
          Renovar
        </v-btn>
      </UiParentCard>
    </v-col>
  </v-row>
</template>

<style scoped>
.text-h6 {
  font-size: 1.25rem;
}
.text-body-1 {
  font-size: 1rem;
}
.font-weight-bold {
  font-weight: 600;
}
</style>
