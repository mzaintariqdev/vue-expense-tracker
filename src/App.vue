<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <!-- + sign added to make sure these are passed as numbers -->
    <IncomeExpenses :income="+income" :expenses="+expenses" />
    <!-- passing a prop transactions -->
    <TransactionList :transactions="transactionList" @transactionDeleted="handleTransactionDelete" />
    <!-- when we submit the form this event is emitted we call this function receiving the response-->
    <AddTransaction @transactionSubmitted="handleTransactionSubmit"/>
  </div>
</template>
<!-- Composition api -->
<script setup>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import { ref, computed, onMounted } from 'vue';
  import { useToast } from 'vue-toastification';


  onMounted(()=>{
    const savedTransactions = JSON.parse(localStorage.getItem('transactionList'));

    if(savedTransactions){
      transactionList.value=savedTransactions;
    }
  })
  /// use ref to make sure this array is reactive (will update)
  const transactionList = ref([])
const toast = useToast()
  // Get total 
const total= computed(()=>{
  return transactionList.value.reduce((acc, transaction)=>{
    return acc+transaction.amount;
  }, 0)
})

  // Get income
  const income = computed(()=>{
    return transactionList.value
    .filter((transaction)=>transaction.amount > 0)
    .reduce((acc, transaction)=>{
      return acc+transaction.amount;
    },0).toFixed(2)
  })

  // Get expenses
  const expenses = computed(()=>{
    return transactionList.value
    .filter((transaction)=>transaction.amount < 0)
    .reduce((acc, transaction)=>{
      return acc+transaction.amount;
    },0).toFixed(2)
  })


  // Add Transactions

  const handleTransactionSubmit = (transactionData)=>{
    transactionList.value.push({
      id: transactionList.value.length+1,
      text: transactionData.text,
      amount: transactionData.amount,
    })
    saveToLocalStorage();
    toast.success("Transaction added")
  }
  const handleTransactionDelete = (id)=>{
    transactionList.value = transactionList.value.filter((transaction)=>transaction.id !==id);
    saveToLocalStorage();
    toast.success("Transaction Deleted")

  }

  // save to localstorage

  const saveToLocalStorage =()=>{
    localStorage.setItem('transactionList', JSON.stringify(transactionList.value));
  }
</script>
<!-- options api syntax -->

<!-- <script>
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpenses from './components/IncomeExpenses.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';

  export default {
    components: {
      Header,
      Balance,
      IncomeExpenses,
      TransactionList,
      AddTransaction
    }
  }
</script> -->