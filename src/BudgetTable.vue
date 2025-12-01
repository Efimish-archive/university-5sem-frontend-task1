<script setup>
import { ref, computed } from 'vue'

import BudgetSummary from './BudgetSummary.vue'

defineEmits({
  remove: null,
})

const { transactions } = defineProps({
  transactions: null,
})

const sorting = ref('no')

function switchSorting() {
  switch (sorting.value) {
    case 'no':
      sorting.value = 'asc'
      break
    case 'asc':
      sorting.value = 'desc'
      break
    case 'desc':
      sorting.value = 'no'
      break
  }
}

const sortedTransactions = computed(() => {
  const newTransactions = [...transactions]
  if (sorting.value === 'asc') {
    newTransactions.sort(
      (a, b) => (a.type === 'income' ? 1 : -1) * a.value - (b.type === 'income' ? 1 : -1) * b.value,
    )
  }
  if (sorting.value === 'desc') {
    newTransactions.sort(
      (a, b) => (b.type === 'income' ? 1 : -1) * b.value - (a.type === 'income' ? 1 : -1) * a.value,
    )
  }
  return newTransactions
})
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4">
    <h2 class="text-lg font-semibold mb-3">Операции</h2>

    <table class="w-full text-left border-collapse">
      <thead>
        <tr class="border-b">
          <th class="p-2">Название</th>
          <th class="p-2">Тип</th>
          <th class="p-2">
            <button @click="switchSorting">Сумма</button>
          </th>
          <th class="p-2">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr class="border-b" v-for="transation in sortedTransactions" :key="transation.id">
          <td class="p-2">{{ transation.name }}</td>

          <td class="p-2 text-green-600 font-medium" v-if="transation.type === 'income'">Доход</td>
          <td class="p-2 text-red-600 font-medium" v-else>Расход</td>

          <td class="p-2" v-if="transation.type === 'income'">+{{ transation.value }}</td>
          <td class="p-2" v-else>-{{ transation.value }}</td>

          <td class="p-2 text-sm text-red-500 cursor-pointer">
            <button @click="$emit('remove', transation.id)">Удалить</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <BudgetSummary :transactions="transactions" />
</template>
