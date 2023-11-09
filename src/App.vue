<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="income" :expense="expense" />
    <TransactionList :transactions="transactions" />
    <AddTransactions />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransactions from "./components/AddTransactions.vue";
import { computed, ref } from "vue";

const transactions = ref([
  { id: 1, text: "Pizza", amount: -560 },
  { id: 2, text: "Coffee", amount: 130 },
  { id: 3, text: "Freelance", amount: -400 },
  { id: 4, text: "Dept Pay", amount: 300 },
]);

// Get Total
const total = computed(() => {
  return transactions.value.reduce((ac, trans) => {
    return ac + trans.amount;
  }, 0);
});
// console.log(total);

// Get Income
const income = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount > 0)
    .reduce((ac, trans) => {
      return ac + trans.amount;
    }, 0)
    .toFixed(2);
});
// Get Expenses
const expense = computed(() => {
  return transactions.value
    .filter((trans) => trans.amount < 0)
    .reduce((ac, trans) => {
      return ac + trans.amount;
    }, 0)
    .toFixed(2);
});
</script>
