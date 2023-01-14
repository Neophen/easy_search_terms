<script setup lang="ts" name="SearchTerm">
import { computed } from "vue";

const props = defineProps<{
  term: string;
  clickedTerms: string[];
}>();

const emit = defineEmits<{
  (event: "click", term: string): void;
}>();

const googleLink = computed(
  () => `https://www.google.com/search?q=${props.term}`
);

const hasBeenClicked = computed(() => {
  return props.clickedTerms.includes(props.term);
});
</script>

<template>
  <a
    :href="googleLink"
    target="_blank"
    @click="emit('click', term)"
    rel="noopener noreferrer"
    class="rounded-md border px-4 py-2"
    :class="{
      'border-gray-600 bg-white hover:bg-gray-100': !hasBeenClicked,
      'border-gray-300 bg-gray-100 text-gray-400': hasBeenClicked,
    }"
  >
    {{ term }}
  </a>
</template>
