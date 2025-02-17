<script setup>
import { ref, onMounted } from 'vue';
import AddTransaction from './components/AddTransaction.vue';
import TransactionList from './components/TransactionList.vue';

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions')) || [];
  transactions.value = savedTransactions;
});

const addTransaction = (transaction) => {
  transactions.value.push(transaction);
  saveToLocalStorage();
};

const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((t) => t.id !== id);
  saveToLocalStorage();
};

const saveToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <AddTransaction @add-transaction="addTransaction" />
        <TransactionList  :transactions="transactions" @delete-transaction="deleteTransaction" />
      </div>
    </div>
 
  </div>
</template>

