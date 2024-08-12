<template>
  <div class="container">
    <h2>Enregistrement des ventes</h2>
    <form @submit.prevent="submitForm">
      <div class="mb-3">
        <label for="reference" class="form-label">Référence</label>
        <input type="text" id="reference" v-model="sale.reference" class="form-control" required>
        <div v-if="errors.reference" class="text-danger">{{ errors.reference }}</div>
      </div>
      <div class="mb-3">
        <label for="designation" class="form-label">Désignation</label>
        <input type="text" id="designation" v-model="sale.designation" class="form-control" required>
        <div v-if="errors.designation" class="text-danger">{{ errors.designation }}</div>
      </div>
      <div class="mb-3">
        <label for="quantity" class="form-label">Quantité Vendue</label>
        <input type="number" id="quantity" v-model="sale.quantity" class="form-control" required>
        <div v-if="errors.quantity" class="text-danger">{{ errors.quantity }}</div>
      </div>
      <div class="mb-3">
        <label for="price" class="form-label">Prix de Vente</label>
        <input type="number" id="price" v-model="sale.price" class="form-control" required>
        <div v-if="errors.price" class="text-danger">{{ errors.price }}</div>
      </div>
      <button type="submit" class="btn btn-primary">Enregistrer la vente</button>
    </form>

    <h3 class="mt-5">Ventes enregistrées</h3>
    <table class="table table-striped mt-3" v-if="sales.length">
      <thead>
        <tr>
          <th>Référence</th>
          <th>Désignation</th>
          <th>Quantité</th>
          <th>Prix</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(sale, index) in sales" :key="index">
          <td>{{ sale.reference }}</td>
          <td>{{ sale.designation }}</td>
          <td>{{ sale.quantity }}</td>
          <td>{{ sale.price }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>Aucune vente enregistrée.</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const sale = ref({
  reference: '',
  designation: '',
  quantity: null,
  price: null,
});

const sales = ref([]);
const errors = ref({});

const validateForm = () => {
  errors.value = {};

  if (!sale.value.reference) {
    errors.value.reference = 'La référence est obligatoire.';
  }
  if (!sale.value.designation) {
    errors.value.designation = 'La désignation est obligatoire.';
  }
  if (!sale.value.quantity || sale.value.quantity <= 0) {
    errors.value.quantity = 'La quantité doit être supérieure à 0.';
  }
  if (!sale.value.price || sale.value.price <= 0) {
    errors.value.price = 'Le prix doit être supérieur à 0.';
  }

  return Object.keys(errors.value).length === 0;
};

const submitForm = () => {
  if (validateForm()) {
    sales.value.push({ ...sale.value });
    sale.value.reference = '';
    sale.value.designation = '';
    sale.value.quantity = null;
    sale.value.price = null;
  }
};
</script>

<style scoped>
.container {
  margin-top: 20px;
}
</style>
