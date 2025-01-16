<template>
  <div
    v-if="isOpen"
    class="fixed inset-0 bg-gray-500 bg-opacity-50 flex justify-center items-center"
  >
    <div class="bg-white p-6 rounded-lg shadow-lg max-w-sm w-full">
      <div class="flex justify-between items-center flex-col">
        <div class="flex flex-col">
          <span class="text-lg font-semibold">
            Name: {{ peopleDetails?.name }}
          </span>
          <span class="text-lg font-semibold">
            Date of Birth: {{ peopleDetails?.date }}
          </span>
          <span class="text-lg font-semibold">
            Gender: {{ peopleDetails?.gender }}
          </span>
          <span class="text-lg font-semibold">
            Country: {{ peopleDetails?.country }}
          </span>
          <span class="text-lg font-semibold">
            Email: {{ peopleDetails?.email }}
          </span>
        </div>
        <button
          @click="closeDialog"
          class="absolute text-lg font-bold text-gray-500 right-0"
        >
          ×
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue';

const props = defineProps({
  peopleDetails: {
    type: Object,
    default: null,
  },
  isOpen: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits();

const localIsOpen = ref(props.isOpen);

watch(
  () => props.isOpen,
  (newVal) => {
    localIsOpen.value = newVal;
  }
);

const closeDialog = () => {
  emit('update:isOpen', false);
};
</script>

<style scoped>
.bg-gray-500 {
  background-color: rgba(0, 0, 0, 0.5);
}
.bg-white {
  background-color: white;
}
</style>
