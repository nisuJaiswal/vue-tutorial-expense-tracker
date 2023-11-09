<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="trans in transactions"
      :key="trans.id"
      :class="trans.amount > 0 ? 'plus' : 'minus'"
    >
      {{ trans.text }} <span> ₹ {{ trans.amount }}</span
      ><button class="delete-btn">x</button>
    </li>
    <!-- <li class="minus">
      Cash <span>-$400</span><button class="delete-btn">x</button>
    </li> -->
  </ul>
</template>
<script setup>
import { defineProps } from "vue";

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
</script>
