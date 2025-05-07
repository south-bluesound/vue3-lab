<script setup>
    defineProps({
        description: {
            type: String,
            default: '',
            required: true
        },
        link: {
            type: String,
            default: null,
            required: false
        }
    })
</script>

<template>
    <p>{{ description }}</p>
    <a v-if="link" :href="link">リンク</a>
</template>