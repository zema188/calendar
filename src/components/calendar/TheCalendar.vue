<template>
    <div class="calendar">
        <div class="calendar__header">
            <button class="calendar__header-btn btn-prev"
                @click="prevMonth()"
            >
                <icon-arrow/>
            </button>

            <div>
                {{ displayMonth }} {{ displayYear }}
            </div>

            <button class="calendar__header-btn btn-next"
                @click="nextMonth()"
            >
                <icon-arrow/>
            </button>
        </div>
        <div class="calendar__content">
            <calendar-week
                :week="week[lang]"
            />
            <calendar-month
                :daysOfMonth="daysOfMonth"
                :dayMonthStart=dayMonthStart
                :lang="lang"
                v-model:activeDay="activeDay"
                @update:activeDay="changeActiveDay()"
            />
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, compile, computed } from 'vue';

import IconArrow from '../icons/IconArrow.vue'
import CalendarWeek from './CalendarWeek.vue';
import CalendarMonth from './CalendarMonth.vue';

const props = defineProps({
    initDate: {
        type: [null, String],
        required: false
    },

    lang: {
        type: [null, String],
        default: 'ru',
        required: false
    },
})

const emits = defineEmits(['pickedDay'])

const week = ref({
    ru: ['Пн','Вт','Ср','Чт','Пт','Сб','Вс'],
    en: ['Sun','Mon','Tue','Wed','Thu','Fri','Sat']
})

const months = ref({
    ru: ['Январь','Февраль','Март','Апрель','Май','Июнь','Июль','Август','Сентябрь','Октябрь','Ноябрь','Декабрь'],
    en: ['January','February','March','April','May','June','July','August','September','October','November','December']
})

const currentDate = ref({})

const activeDay = ref(null)

const initCalendar = () => {
    if(!props.initDate) {
        const dateObj = new Date()

        currentDate.value.year = dateObj.getFullYear()
        currentDate.value.month = dateObj.getMonth()
        currentDate.value.day = dateObj.getDate()
    } else {
        currentDate.value.year = parseInt(props.initDate.split('-')[0])
        currentDate.value.month = parseInt(props.initDate.split('-')[1]) - 1
        currentDate.value.day = parseInt(props.initDate.split('-')[2])
    }

    activeDay.value = currentDate.value.day
}

const displayMonth = computed(() => {
    return currentDate.value.month !== null ? months.value[props.lang][currentDate.value.month] : ''
})

const displayYear = computed(() => {
    return currentDate.value.year ? currentDate.value.year : ''
})

const daysOfMonth = computed(() => {
    return (currentDate.value.year && currentDate.value.month !== null) ? 32 - new Date(currentDate.value.year, currentDate.value.month, 32).getDate() : null
})

const nextMonth = () => {
    if(currentDate.value.month === 11) {
        currentDate.value.month = 0
        currentDate.value.year += 1
    } else {
        currentDate.value.month += 1
    }

    monthChange()
}

const prevMonth = () => {
    if(currentDate.value.month === 0) {
        currentDate.value.month = 11
        currentDate.value.year -= 1
    } else {
        currentDate.value.month -= 1
    }
    
    monthChange()
}

const monthChange = () => {
    while (activeDay.value > daysOfMonth.value ) {
        activeDay.value -= 1
    }

    currentDate.value.day = activeDay.value
}

const changeActiveDay = () => {
    currentDate.value.day = activeDay.value
    emits('pickedDay', new Date(displayDate.value))
}


const displayDate = computed(() => {
    return `${currentDate.value.year}-${currentDate.value.month}-${currentDate.value.day}`
})

const dayMonthStart = computed(()  => {
    if(currentDate.value.year && currentDate.value.month !== null) {
        let date = new Date(currentDate.value.year, currentDate.value.month, 1)
        return date.getDay()
    }
    return null
})



onMounted(() => {
    initCalendar()
})
</script>

<style lang="scss" scoped>
.calendar {
    border: 1px solid #969696;
    border-radius: 12px;
    padding: 10px;
    width: 302px;
    min-height: 350px;
    &__header {
        display: flex;
        justify-content: space-between;
    }

    &__header-btn {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 20px;
    }

    &__content {

    }
}

.btn-prev {

}
.btn-next {
    transform: rotate(180deg);
}

</style>