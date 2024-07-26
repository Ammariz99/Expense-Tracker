<template>
    <div>
        <h3>Add new Transaction</h3>
        <form id="form" @submit.prevent="onSubmit">
            <div class="form-control">
                <label for="text">Text</label>
                <input type="text" id="text" v-model="text" placeholder="Enter text..." />
            </div>
            <div class="form-control">
                <label for="amount">Amount <br/>
                (negative - expense, positive - income)</label>
                <input type="text" id="amount" v-model="amount" placeholder="Enter amount..." />
            </div>
            <button class="btn">Add transaction</button>
        </form>
    </div>
</template>

<script setup>
/***
 * Import necessary functions from Vue and external libraries
 */
import {ref} from 'vue';
import {useToast} from 'vue-toastification';

/***
 * Define reactive state variables for form inputs
 * - text: Stores the transaction description
 * - amount: Stores the transaction amount
 */
const text =  ref('');
const amount = ref('');

/***
 * Initialize toast notifications
 */
const toast = useToast();

/***
 * Define component events
 * - emit: Used to emit the 'transactionSubmitted' event
 */
const emit =  defineEmits(['transactionSubmitted']);

/***
 * Handle form submission
 * - Check if both fields are filled
 * - Show an error toast if fields are empty
 * - Parse the amount to a float
 * - Emit the 'transactionSubmitted' event with transaction data
 * - Clear the input fields after submission
 */
const onSubmit = () => {
    if(!text.value || !amount.value){
        toast.error('Please fill all fields');
        return;
    }

const transactionData = {
    text: text.value,
    amount: parseFloat(amount.value)

}
emit('transactionSubmitted', transactionData);

    text.value = '';
    amount.value = '';
}
</script>