<template>
    <div class="inc-exp-container">
        <div>
            <h4>Income</h4>
            <p id="money-plus" class="money plus">+${{ income }}</p>
        </div>
        <div>
            <h4>Expense</h4>
            <p id="money-minus" class="money minus">${{ expenses }}</p>
        </div>
    </div>
</template>

<script setup>
// Import the defineProps function from Vue
import { defineProps } from 'vue';


/**
 * Define props for this component
 * - income: Number (required) - Total income value
 * - expenses: Number (required) - Total expenses value
 */

const props = defineProps({
    income: {
        type: Number,
        required: true
    },
    expenses: {
        type: Number,
        required: true
    }
})
</script>