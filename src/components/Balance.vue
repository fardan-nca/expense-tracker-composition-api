<template>
    <h4>Your Balance</h4>
      <h1 id="balance">${{totalAmount}}</h1>
</template>
<script setup>
import { defineProps } from 'vue';
const props = defineProps({
    totalAmount: {
        type: Number,
        required: true
    }
})
</script>