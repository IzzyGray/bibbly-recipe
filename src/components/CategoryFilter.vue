<template>
  <q-select filled :model-value="modelValue" @update:model-value="$emit('update:modelValue', $event)"
    :options="categories" label="Kategorie auswählen" emit-value map-options />
</template>

<script setup lang="ts">
defineProps({
  categories: Array,
  modelValue: String
})

defineEmits(['update:modelValue'])
</script>
