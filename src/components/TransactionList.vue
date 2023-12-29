<template>
          <h3>History</h3>
      <ul id="list" class="list">
        <li 
        v-for="transaction in transactions"
        :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
         {{ transaction.text }}
         <span>{{ transaction.amount }} kr</span>
        <button @click="deletetransition(transaction.id)" class="delete-btn">x</button>
        </li>

      </ul>
</template>

<script setup>
import { defineProps } from "vue";

const emit = defineEmits(['transactiondeleted']);

 const props = defineProps({
    transactions: {
        type: Array,
        required: true,
    },
 });

 const deletetransition = (id) => {
    emit('transactiondeleted', id);
 };

</script>