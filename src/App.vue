<template>
  <Header />
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TransactionList
      :transactions="transactions"
      @dataDeleted="handleDataDeleted"
    />
    <AddTransactions @dataAdded="handleDataAdded" />
  </div>
</template>

<script setup>
import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransactions from "./components/AddTransactions.vue";
import { v4 as uuid } from "uuid";
import { useToast } from "vue-toastification";
import { computed, ref, onMounted } from "vue";

// Similar to useEffect
onMounted(() => {
  const existingData = JSON.parse(localStorage.getItem("transaction"));
  if (existingData) transactions.value = existingData;
});

const toast = useToast();
const transactions = ref([]);

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

// Save to Local Storage
const saveToLS = () => {
  localStorage.setItem("transaction", JSON.stringify(transactions.value));
};

// Add data into Transaction
const handleDataAdded = (transData) => {
  transactions.value.push({
    id: uuid(),
    text: transData.text,
    amount: transData.amount,
  });
  saveToLS();
  toast.success("Transaction added Successfully");
};

// Handle Data Deleted
const handleDataDeleted = (id) => {
  transactions.value = transactions.value.filter((trans) => trans.id !== id);
  saveToLS();
  toast.success("Transaction Deleted Successfully");
};
</script>
