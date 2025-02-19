<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['delete-transaction']);

const filterType = ref('all');

const filteredTransactions = computed(() => {
  if (filterType.value === 'all') {
    return props.transactions;
  } else {
    return props.transactions.filter((t) => t.type === filterType.value);
  }
});

const deleteTransaction = (id) => {
  emit('delete-transaction', id);
};
</script>

<template>
  <div>
    <section class="d-flex m-3">
      <div class="col-2 ">
        <input type="radio" v-model="filterType" value="all" />
        <label for="All">All</label>
      </div>
      <div class="col-2 ">
        <input type="radio" v-model="filterType" value="income" />
        <label for="income">Income</label>
      </div>
      <div class="col-2 ">
        <input type="radio" v-model="filterType" value="expense" />
        <label for="Expense">Expense</label>
      </div>
    </section>
    
    <table class="table table-bordered border-primary">
      <thead>
        <tr>
          <th>Title</th>
          <th>Amount</th>
          <th>Type</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="transaction in filteredTransactions" :key="transaction.id">
          <td>{{ transaction.title }}</td>
          <td
            :class="{ 'text-success': transaction.type === 'income', 'text-danger': transaction.type === 'expense', 'fw-bold': transaction.amount >= 500 }">
            {{ transaction.amount }}
          </td>
          <td>{{ transaction.type.toUpperCase() }}</td>
          <td>
            <button @click="deleteTransaction(transaction.id)" class="btn btn-danger btn-sm">Delete</button>
          </td>
        </tr>
        <tr v-if="filteredTransactions.length === 0">
          <td colspan="4">
            <p class="text-center p-3 fs-2">No transactions recorded yet !!</p>
          </td>
        </tr>
      </tbody>
    </table>


  </div>
</template>
