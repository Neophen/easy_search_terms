<script setup lang="ts" name="RegenerateButton">
import ArrowPath from './ArrowPath.vue';


const emit = defineEmits<{
    (e: 'click'): void
}>()

</script>

<template>
    <button type="button"
        class="inline-flex items-center rounded-full border border-transparent bg-gray-600 p-1 text-white shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 gap-2"
        @click="emit('click')">
        <ArrowPath />
        <span class="hidden md:block pr-2">Regenerate</span>
    </button>
</template>
