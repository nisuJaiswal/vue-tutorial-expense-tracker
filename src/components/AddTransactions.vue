<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <!-- v-model will be used to access the values -->
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount"
        >Amount <br />
        (negative - expense, positive - income)</label
      >
      <input
        type="text"
        id="amount"
        placeholder="Enter amount..."
        v-model="amount"
      />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import { useToast } from "vue-toastification";

const text = ref("");
const amount = ref("");
const toast = useToast();

// Initalizing the emitter, name will be used to emit the event
const emit = defineEmits(["dataAdded"]);

const handleSubmit = () => {
  // If not all values provided, then display toast
  if (!text.value || !amount.value) {
    return toast.error("Fill Both the fields");
  }

  // Required Data for emitter (Similar to Schema)
  const transactionFields = {
    text: text.value,
    amount: parseFloat(amount.value),
  };

  // Calling the event emitter
  emit("dataAdded", transactionFields);
};
</script>
