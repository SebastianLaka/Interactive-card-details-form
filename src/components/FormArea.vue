<script setup>
const props = defineProps({
    formData: {
        type: Object,
        required: true,
    }
});
</script>
<template>
    <div class="form-group-wrapper">
        <slot></slot>
    </div>
</template>