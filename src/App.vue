<template>
    <Header />
    <div class="container">
        <Balance :total="+total"/>
        <IncomExepenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions"
         @transactiondeleted="handletransactiondeleted" />
        <AddTransaction @transactionSubmitted="handletransactionSubmitted" />
    </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomExepenses from './components/IncomExepenses.vue';
import TransactionList from './components/TransactionList.vue';
import Transaction from './components/AddTransaction.vue';

import {useToast} from 'vue-toastification';

import {ref, computed, onMounted} from 'vue';

const toast = useToast();

import AddTransaction from './components/AddTransaction.vue';

const transactions = ref ([]);

onMounted(() => {
    const savedTranactions = JSON.parse(localStorage.getItem('transactions'));    

    if(savedTranactions) {
        transactions.value = savedTranactions;
    }
});

const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0);
});

const income = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const expenses = computed(() => {
    return transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0)
    .toFixed(2);
});

const handletransactionSubmitted = (transactionData) => {
    transactions.value.push({
        id: generateUniqueId(),
        text: transactionData.text,
        amount: transactionData.amount,
    });

    saveTransactionsToLocalStorage();
    
    toast.success('transaction added')
};

const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

const handletransactiondeleted = (id) => {
    transactions.value  = transactions.value.filter((transaction) => 
    transaction.id !== id);

    saveTransactionsToLocalStorage();

    toast.success('transaction deleted');
};
    

const saveTransactionsToLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

</script>

