<template>
    <div class="calendar__month" v-if="props.dayMonthStart">
        <div class="hidden-day"
            v-for="item of hiddenDays" :key="item"
        ></div>
        <Month-Item
            v-for="day of daysOfMonth" :key="day"
            :day="day"
            :isActive="day === activeDay"
            @changeActiveDay="emits('update:activeDay', $event)"
        >
            {{ day }}
        </Month-Item>
    </div>
</template>

<script setup>
import { computed } from 'vue'

import MonthItem from './MonthItem.vue';

const props = defineProps({
    daysOfMonth: {
        type: [null, Number],
        required: false
    },

    activeDay: {
        type: [null, Number],
        required: true
    },

    dayMonthStart: {
        type: [null, Number],
        required: true,
    },

    lang: {
        type: [null, String],
        required: true
    },
})

const hiddenDays = computed(() => {
    if(props.lang === 'en') return props.dayMonthStart

    return props.dayMonthStart - 1
})

const emits = defineEmits(['update:activeDay'])

</script>

<style lang="scss" scoped>
.calendar {
    &__month {
        display: flex;
        flex-wrap: wrap;
    }
}

.hidden-day {
    width: 40px;
    height: 40px;
}
</style>