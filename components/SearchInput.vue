<template>
    <div>
        <input type="text" :value="value" @input="$emit('input', $event.target.value)">
        <button type="button" @click="$emit('search')">search</button>
    </div>
</template>
<script>
export default {
    props: {
        value: {
            type: String,
            default: () => '',
        },
    },
}
</script>
<style scoped>
    
</style>