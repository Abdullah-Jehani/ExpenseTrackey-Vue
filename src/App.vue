<template>
  <Header />
  <div class="container">
      <Balance :Total="+Total" />
      <IncomeExpenses :income="+income" :expenses="+expenses" />
      <TransactionList :transactions="transactions" @TransactionDeleted="handleTransactionDelete" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
      </div>
    
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed } from 'vue';
import {  useToast } from 'vue-toastification';
const transactions =  ref([       
  ]);
  const toast = useToast();

  // GetTotal
  const Total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.cost;
    }, 0).toFixed(2);
  });

  // Get Income
  const income = computed(() => {
    return transactions.value.
    filter((transaction) => transaction.cost > 0)
    .reduce((acc, transaction) => {
        return acc + transaction.cost;
    }, 0).toFixed(2);
  });

   // Get Expenses
   const expenses = computed(() => {
    return transactions.value.
    filter((transaction) => transaction.cost < 0)
    .reduce((acc, transaction) => {
        return acc + transaction.cost;
    }, 0).toFixed(2);
  });

  // Add transaction 

 const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId() , 
      text: transactionData.text , 
      cost: transactionData.amount
    });
  };

  const generateUniqueId = () => {
    return Math.floor(Math.random() * 10000);
  };

  // Delete Transaction

  const handleTransactionDelete = (id) => {
    transactions.value = transactions.value.filter( (transaction) => transaction.id !== id);
    toast.success('Transaction Deleted Successfully !');
  }



</script>