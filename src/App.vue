<template>
  <Header />
  <div class="container">
    <Balance :totalAmount='+total' />
    <IncomeExpenses :income='+income' :expenses='+expenses' />
    <TransactionList :transactions='transactions' @transactionDelete="handleDeleteTransactions" />
    <AddTransaction @transactionSubmited="handleTransaction" />
  </div>
</template>
<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import { ref, computed, onMounted } from 'vue';
import { useToast } from 'vue-toastification';
const transactions = ref([]);
const toast = useToast();
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)
});
onMounted(() => {
  const storedTransactions = JSON.parse(localStorage.getItem('transactionsStoreData'));
  if(storedTransactions) {
    transactions.value = storedTransactions;
  }
  
})
const income = computed(() => {
  return transactions.value.
    filter((transaction) => (transaction.amount > 0))
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0);
});
const expenses = computed(() => {
  return transactions.value.
    filter((transaction) => (transaction.amount < 0))
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0);
});
const handleTransaction = (transactionData) => {
  transactions.value.push(transactionData);
  toast.success('Transctions added successfully ');
  handleStoreTransactions();
}
const handleDeleteTransactions = (id) => {
  transactions.value = transactions.value.filter((value) => (value.id !== id));
  toast.success('transaction remove successfully');
  handleStoreTransactions();
}
const handleStoreTransactions = () => {
  localStorage.setItem('transactionsStoreData', JSON.stringify(transactions.value));
}
</script>