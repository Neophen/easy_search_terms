<script setup lang="ts" name="ToggleButton">
import { ref, watch } from 'vue'

import XMarkIcon from './XMarkIcon.vue'
import PlusIcon from './PlusIcon.vue'

const props = defineProps<{
    modelValue: boolean
}>()

const emit = defineEmits<{
    (e: 'update:modelValue', value: boolean): void
}>()

const isOpen = ref(props.modelValue)

const toggle = () => {
    isOpen.value = !isOpen.value
    emit('update:modelValue', isOpen.value)
}

watch(
    () => props.modelValue,
    (value) => {
        isOpen.value = value
    },
    { immediate: true }
)
</script>

<template>
    <button type="button"
        class="inline-flex items-center rounded-full border border-transparent bg-gray-600 p-1 text-white shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 gap-2"
        @click="toggle">
        <XMarkIcon v-if="isOpen" />
        <PlusIcon v-else />
        <span class="hidden md:block pr-2">
            {{ isOpen? 'Hide Inputs': 'Show Inputs' }}
        </span>
    </button>
</template>
