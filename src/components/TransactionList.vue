<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="trans in transactions"
      :key="trans.id"
      :class="trans.amount > 0 ? 'plus' : 'minus'"
    >
      {{ trans.text }} <span> ₹ {{ trans.amount }}</span
      ><button class="delete-btn" @click="deleteTransaction(trans.id)">
        x
      </button>
    </li>
  </ul>
</template>
<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(["dataDeleted"]);

// Delete Transaction
const deleteTransaction = (id) => {
  emit("dataDeleted", id);
};
</script>
