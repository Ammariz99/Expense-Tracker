<template>
  <div>
<Header/>
  <div class="container">
    <Balance :total="total" />
    <IncomeExpenses :income="+income" :expenses="+expenses"/>
    <TransactionList :transactions="transactions" @transactionDeleted="handleDeleteTransaction"/>
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
  </div>

</div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';
import {useToast} from 'vue-toastification';


// import { RouterLink, RouterView } from 'vue-router'
/** import the ref for reactivty
 *  import life-cycle hook onMounted 
 */
import { computed, ref, onMounted } from 'vue';

/***
 * Initialize toast notifications
 */
const toast = useToast();

/** Create a global state variable for transactions
 * All Trasactions will be stored in this array
 */
const  transactions = ref([]);


/**=====================Life-Cycle Hooks Start================== */
onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

/**=====================Life-Cycle Hooks End================== */


/**==================Computed Property Start=================*/
/**
 * Compute the total balance
 * - Sum the amount of all transactions using the reduce method
 * - reduce method iterates over each transaction and accumulates the total amount
 * - Access the reactive value using transactions.value
 */

const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount;
  },0)
});

/**
 * Compute the total income
 * - Filter transactions to include only those with a positive amount (income)
 * - Use the reduce method to sum the amounts of the filtered transactions
 * - Access the reactive value using transactions.value
 */

const income = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount > 0) // Filter out non-income transactions
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
});

/**
 * Compute the total expenses
 * - Filter transactions to include only those with a negative amount (expenses)
 * - Use the reduce method to sum the amounts of the filtered transactions
 * - Access the reactive value using transactions.value
 */

const expenses = computed(() => {
  return transactions.value.filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount;
    }, 0).toFixed(2);
});
/**==================Computed Property End===================*/



/***
 * Handle new transaction submissions
 * - Add the new transaction to the transactions array
 * - Save the updated trasaction array to local-Storage
 * - Show a success toast notification
 * 
 * @param {Object} transactionData - Contains text and amount of the new transaction
 * @param {string} transactionData.text - The description of the transaction
 * @param {number} transactionData.amount - The amount of the transaction
 */
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })
  saveTransactionToLocalStorage();
  toast.success('Transaction added')
}

/***
 * Generate a unique ID for a new transaction
 * - Returns a random number between 0 and 100000
 * @returns {number} Unique identifier
 */
const generateUniqueId = () => {
  return Math.floor(Math.random() * 100000);
}

/***
 * Handle transaction deletion
 * - Filter out the transaction with the specified id from the transactions array
 * - Update the transactions array with the remaining transactions
 * - Save the updated transactions array to local storage
 * - Show a success toast notification
 * 
 * @param {number} id - The id of the transaction to be deleted
 */
const handleDeleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id);
  saveTransactionToLocalStorage();
  toast.success('Transaction Deleted');
}

/***
 * Save transactions to local storage
 * - Convert transactions array to a JSON string
 * - Save the JSON string to local storage
 */
const saveTransactionToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
}
</script>



