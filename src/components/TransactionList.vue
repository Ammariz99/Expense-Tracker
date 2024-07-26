<template>
    <div>
<h3>History</h3>
<ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus' : 'plus'">
        {{ transaction.text }} <span>${{ transaction.amount }}</span>
        <button class="delete-button" @click="deleteTransaction(transaction.id)">x</button>
    </li>
   
</ul>    
</div>
</template>

<script setup>
/**==================Props Start========================== */
// Import the defineProps function from Vue
import { defineProps } from 'vue';

const emit = defineEmits(['transactionDeleted'])
/**
 * Define props
 * - transactions: Array (required)
 */
const props = defineProps({
    transactions: {
    type: Array,
    required: true,
    },
});
/**==================Props End========================== */


/***
 * Emit a transaction deletion event
 * - Emits the 'transactionDeleted' event with the specified transaction id
 * 
 * @param {number} id - The id of the transaction to be deleted
 */
const deleteTransaction = (id) => {
    emit('transactionDeleted', id)
}

</script>
