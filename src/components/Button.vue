<template>
  <button
    :class="buttonClass"
    @click="handleClick"
  >
    <slot></slot>
  </button>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  variant: {
    type: String,
    default: 'primary',
  },
});

const emit = defineEmits(['click']);

const buttonClass = computed(() => {
  return {
    'px-4 py-2 font-bold rounded-md transition-colors': true,
    'bg-blue-500 text-white hover:bg-blue-600': props.variant === 'primary',
    'bg-gray-300 text-gray-800 hover:bg-gray-400': props.variant === 'secondary',
    'bg-red-500 text-white hover:bg-red-600': props.variant === 'danger',
  };
});

const handleClick = (event) => {
  emit('click', event);
};
</script>