<template>
    <div class="calendar__week">
        <div class="calendar__week-btn"
            v-for="item of week" :key="item"
        >
            {{ item }}
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    week: {
        type: Array,
        required: true
    }
})
</script>

<style lang="scss" scoped>
.calendar {

    &__week {
        display: flex;
        justify-content: space-between;
    }

    &__week-btn {
        width: 40px;
        height: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
}

</style>