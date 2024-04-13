<template>
  <a :href="`/user/${id}/edit`" class="text-indigo-600 hover:text-indigo-500">Edit</a>
</template>

<script setup>
const props = defineProps({
  id: {
    type: Number,
    require: true
  }
})
</script>