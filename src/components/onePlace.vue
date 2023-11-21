<template>
    <li style="margin: 12px auto;">
        {{ obj.unrestricted_value }}
    </li>
</template>

<script>

export default {
    props: {
        obj: {
            type: Object,
            required: true,
        }
    }
}

</script>

<style scoped>

</style>