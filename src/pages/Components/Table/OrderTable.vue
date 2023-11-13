<template>
  <q-select
    filled
    v-model="classification"
    :options="options"
    class="q-ml-sm"
    dense
    options-dense
  >
    <template v-slot:prepend>
      <span class="text-subtitle2">分類:</span>
    </template>
  </q-select>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
const classification = ref('最新');
const options = computed(() => {
  return classification.value == '最新' ? ['舊'] : ['最新'];
});
</script>
