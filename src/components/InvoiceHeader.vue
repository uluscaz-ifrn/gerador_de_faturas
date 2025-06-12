<script setup>
const props = defineProps({
  clientName: String,
});
// Define o emit para atualizar o nome do cliente
const emit = defineEmits(["update:clientName"]);
</script>

<template>
  <div class="mb-3">
    <label class="form-label">Nome do Cliente:</label>
    <input
      type="text"
      :value="clientName"
      @input="emit('update:clientName', $event.target.value)"
      class="form-control"
    />
  </div>
</template>
