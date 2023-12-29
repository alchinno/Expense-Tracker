<template>
    <h4>your Balance</h4>
    <h1 id="balance"> {{ total }} kr</h1>
</template>

<script setup>
 import { defineProps } from 'vue';

 const props = defineProps({
    total: {
        type:Number,
        required: true,
    },
 });
</script>