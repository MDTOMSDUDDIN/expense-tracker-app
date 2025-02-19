<script setup>
import { ref } from 'vue';

const title = ref('');
const amount = ref(0);
const type = ref('income');
const amountError = ref('');
const lastId = ref(0);

const emit = defineEmits(['add-transaction']);

const FormSubmit = () => {
  if (amount.value <= 0) {
    amountError.value = 'Amount must be greater than 0.';
    return;
  }

  lastId.value++;

  const newTransaction = {
    id: lastId.value, 
    title: title.value,
    amount: parseFloat(amount.value),
    type: type.value,
  };

  emit('add-transaction', newTransaction);

  title.value = '';
  amount.value = 0;
  type.value = 'income';
  amountError.value = '';
};
</script>

<template>
  <div>
    <form @submit.prevent="FormSubmit" class="d-flex">
      <div class="col-3 p-1">
        <input type="text" class="form-control" id="title" placeholder="Title" v-model="title" required />
      </div>
      <div class="col-3 p-1">
        <input type="number" class="form-control" placeholder="Amount" id="amount" v-model="amount" required />
        <small v-if="amountError" class="text-danger">{{ amountError }}</small>
      </div>
      <div class="col-3 p-1">
        <select class="form-select" id="type" v-model="type" required>
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
      <div class="col-3 p-1">
        <button type="submit" class="btn btn-primary w-100">Add</button>
      </div>
    </form>
  </div>
</template>
