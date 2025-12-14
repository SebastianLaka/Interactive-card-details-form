<script setup></script>
<template>
    <button class="form-button"><slot/></button>
</template>
<style lang="scss" scoped>
    @use '../assets/sass/colors' as *;
    .form-button{
        background-color: $purple-950;
        color: $white;
        padding: .75em 1.5em;
        border-radius: .4em;
        grid-column: 1/4;
    }
</style>