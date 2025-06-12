<template>
  <main class="container py-4" ref="invoiceRef">
    <h1 class="h2 fw-bold mb-4">Gerador de Faturas</h1>

    <InvoiceHeader v-model:clientName="clientName" />
    <InvoiceTable
      v-model:items="items"
      :showButton="showGenerateButton"
      :remove-add-btn="showGenerateButton"
    />

    <div class="text-end mt-4">
      <p class="fs-4 fw-semibold">Total: R$ {{ total.toFixed(2) }}</p>
    </div>
  </main>
  <button class="btn btn-primary mt-3" @click="generatePDF">Gerar PDF</button>
</template>

<script setup>
import { ref, computed } from "vue";
import InvoiceHeader from "./components/InvoiceHeader.vue";
import InvoiceTable from "./components/InvoiceTable.vue";
import html2pdf from "html2pdf.js";

const clientName = ref("");
const items = ref([{ name: "", quantity: 1, price: 0 }]);
const showGenerateButton = ref(true);
const invoiceRef = ref(null);

// Funçao para adicionar um novo item à fatura
const total = computed(() =>
  items.value.reduce((acc, item) => acc + item.quantity * item.price, 0),
);

// Função para gerar o PDF da fatura
const generatePDF = () => {
  const element = invoiceRef.value;
  if (!element) {
    console.warn("Elemento para PDF não encontrado");
    return;
  }

  // Configurações do html2pdf
  const opt = {
    margin: 0.5,
    filename: `fatura-${clientName.value || "cliente"}.pdf`,
    image: { type: "jpeg", quality: 0.98 },
    html2canvas: { scale: 1 },
    jsPDF: { unit: "in", format: "letter", orientation: "portrait" },
  };
  showGenerateButton.value = false;

  // Gerar o PDF
  html2pdf()
    .set(opt)
    .from(element)
    .save()
    .then(() => {
      showGenerateButton.value = true;
      console.log("PDF gerado com sucesso!");
    })
    .catch((err) => console.error("Erro ao gerar PDF:", err));
};
</script>

<style scoped>
body {
  font-family: sans-serif;
}
</style>
