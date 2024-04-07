<template>
    <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions" v-bind:key="transaction.id"
        v-bind:class="transaction.amount < 0 ? 'minus': 'plus'">
          {{transaction.text}} <span>${{transaction.amount}}</span><button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>


<script setup>
  import { defineProps } from 'vue';

  const emit = defineEmits(['transactionDeleted']);
  const props = defineProps({
    transactions: {
      type: Array,
      required: true,
    }
  })

  const deleteTransaction=(id)=>{
    emit('transactionDeleted',  id);
  }
</script>

<!-- composition api -->
<!-- 
<script setup>
    const transactions = [
          {id: 1, text: "Flower", amount: -19.90},
          {id: 2, text: "Salary", amount: 290},
        ]

</script> -->


<!-- option api syntax -->
<!-- <script>
  export default {
   setup(){
    const transactions = [
          {id: 1, text: "Flower", amount: -19.90},
          {id: 2, text: "Salary", amount: 290},
        ]

        return {
          transactions
        }
   }
  }
</script> -->
