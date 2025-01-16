<template>
  <div
    class="flex border-[1px] w-full p-8 space-x-8 shadow-md hover:border-cyan-500 hover:shadow-cyan-500 hover:text-cyan-500 cursor-pointer"
    @click="handleClick"
  >
    <div class="flex-1 truncate overflow-hidden whitespace-nowrap text-left">
      {{ date }}
    </div>
    <div class="flex-[2] truncate overflow-hidden whitespace-nowrap text-left">
      {{ name }}
    </div>
    <div class="flex-1 truncate overflow-hidden whitespace-nowrap text-left">
      {{ gender }}
    </div>
    <div class="flex-1 truncate overflow-hidden whitespace-nowrap text-left">
      {{ country }}
    </div>
    <div class="flex-[2] truncate overflow-hidden whitespace-nowrap text-left">
      {{ email }}
    </div>
  </div>
</template>
<script setup>
const props = defineProps({
  date: {
    type: String,
  },
  name: {
    type: String,
  },
  gender: {
    type: String,
  },
  country: {
    type: String,
  },
  email: {
    type: String,
  },
});

const emit = defineEmits();

const handleClick = () => {
  const allProps = {
    date: props.date,
    name: props.name,
    gender: props.gender,
    country: props.country,
    email: props.email,
  };

  emit('user-clicked', allProps);
};
</script>

<style></style>
