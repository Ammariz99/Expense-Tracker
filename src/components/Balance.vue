<template>
    <div>
        <h4>Your Balance</h4>
        <h1 id="balance">${{ total }}</h1>
    </div>
</template>

<script setup>
// Import the defineProps function from Vue
import {defineProps} from 'vue';

/** 
 * Define props 
 * - total: Number (required) 
 */
const props = defineProps({
    total: {
        type: Number,
        required: true
    }
})
</script>