<script setup lang="ts">
defineProps<{
  senhorSerigueijo: Array<string>,
  isOpen: boolean,
  name: string
}>()

defineEmits<{
  (e: 'close'): void
  (e: 'delete'): void
}>()
</script>

<template>
  <div>
    <VDialog :model-value="isOpen" max-width="400" @update:model-value="$emit('close')">
      <VCard title="Confirm Delete">
        <VCardText>
          Are you sure you want to delete <strong>{{ name }}</strong>?
        </VCardText>
        <VCardActions>
          <VSpacer />
          <VBtn @click="$emit('close')" color="secondary">Cancel</VBtn>
          <VBtn @click="$emit('delete')" color="error">Delete</VBtn>
        </VCardActions>
      </VCard>
    </VDialog>
  </div>
</template>
