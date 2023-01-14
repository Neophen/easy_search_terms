<script setup lang="ts" name="TextArea">
import { computed } from "vue";

const props = withDefaults(
  defineProps<{
    modelValue: string;
    id: string;
    name: string;
    rows?: number;
  }>(),
  {
    rows: 4,
  }
);

const emit = defineEmits<{
  (e: "update:modelValue", value: string): void;
}>();

const value = computed({
  get() {
    return props.modelValue;
  },
  set(value: string) {
    emit("update:modelValue", value);
  },
});
</script>
<template>
  <div>
    <label :for="id" class="block text-sm font-medium text-gray-700">
      {{ name }}
    </label>
    <div class="mt-1">
      <textarea
        :rows="rows"
        :name="id"
        :id="id"
        v-model="value"
        class="block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm"
      />
    </div>
  </div>
</template>
