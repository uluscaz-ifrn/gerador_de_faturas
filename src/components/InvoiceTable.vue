<script setup>
const props = defineProps({
  items: {
    type: Array,
    required: true,
  },
  showButton: Boolean,
  removeAddBtn: Boolean,
});
const emit = defineEmits(["update:items"]);

// Função para adicionar um novo item à tabela
const addItem = () => {
  emit("update:items", [...props.items, { name: "", quantity: 1, price: 0 }]);
};

// Função para remover um item da tabela
const removeItem = (index) => {
  const updated = [...props.items];
  updated.splice(index, 1);
  emit("update:items", updated);
};
</script>

<template>
  <div>
    <table class="table table-bordered mb-4">
      <thead class="table-light">
        <tr>
          <th>Item</th>
          <th>Quantidade</th>
          <th>Preço</th>
          <th>Subtotal</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in props.items" :key="index">
          <td>
            <input
              v-model="props.items[index].name"
              class="form-control"
              placeholder="Nome do item"
            />
          </td>
          <td>
            <input
              type="number"
              v-model.number="props.items[index].quantity"
              class="form-control"
              min="1"
            />
          </td>
          <td>
            <input
              type="number"
              v-model.number="props.items[index].price"
              class="form-control"
              min="0"
              step="0.01"
            />
          </td>
          <td>R$ {{ (item.quantity * item.price).toFixed(2) }}</td>
          <td class="text-center">
            <button
              @click="removeItem(index)"
              class="btn btn-sm btn-danger"
              v-if="showButton"
            >
              Remover
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <button @click="addItem" class="btn btn-success" v-if="removeAddBtn">
      Adicionar Item
    </button>
  </div>
</template>
