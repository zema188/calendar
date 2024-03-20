<template>
    <div class="calendar__month-item"
        :class="{active: isActive}"
        @click="handleClick"
    >
        {{ day }}
    </div>
</template>

<script setup>
const props = defineProps({
    day: {
        type: [null, Number],
        required: false
    },

    isActive: {
        type: Boolean,
        required: true
    },
})

const emits = defineEmits(['changeActiveDay'])

const handleClick = () => {
    emits('changeActiveDay', props.day)
}
</script>

<style lang="scss" scoped>
.calendar {

    &__month-item {
        width: 40px;
        height: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
        cursor: pointer;
        border: 1px solid transparent;
        transition: .2s;
        &.active {
            border-color: rgb(62, 62, 235);
        }
    }
}
</style>