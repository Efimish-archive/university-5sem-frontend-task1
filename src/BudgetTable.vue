<script setup>
import BudgetSummary from './BudgetSummary.vue'

const { transactions } = defineProps({
  transactions: null,
})

defineEmits({
  remove: null,
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
          <th class="p-2">Сумма</th>
          <th class="p-2">Действия</th>
        </tr>
      </thead>
      <tbody>
        <!-- пример статичных строк, у вас тоже должен быть + и - -->
        <!-- <tr class="border-b">
          <td class="p-2">Зарплата</td>
          <td class="p-2 text-green-600 font-medium">Доход</td>
          <td class="p-2">+50000</td>
          <td class="p-2 text-sm text-red-500 cursor-pointer">Удалить</td>
        </tr>
        <tr class="border-b">
          <td class="p-2">Еда</td>
          <td class="p-2 text-red-600 font-medium">Расход</td>
          <td class="p-2">-800</td>
          <td class="p-2 text-sm text-red-500 cursor-pointer">Удалить</td>
        </tr> -->
        <tr class="border-b" v-for="transation in transactions" :key="transation.id">
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
