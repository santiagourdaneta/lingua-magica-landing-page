<template>
  <form @submit.prevent="handleSubmit" class="p-6 bg-white rounded-lg shadow-xl">
    <h2 class="text-2xl font-bold mb-4">¡Contáctanos!</h2>

    <div class="mb-4">
      <Input
        label="Nombre Completo"
        v-model="formData.name"
        :error="errors.name"
        maxlength="50"
      />
    </div>

    <div class="mb-4">
      <Input
        label="Correo Electrónico"
        type="email"
        v-model="formData.email"
        :error="errors.email"
        maxlength="100"
      />
    </div>

    <div class="mb-6">
      <label for="message" class="block text-gray-700 font-medium mb-1">
        Mensaje
        <span class="text-xs text-gray-500 float-right">
          {{ formData.message.length }}/500
        </span>
      </label>
      <textarea
        id="message"
        v-model="formData.message"
        rows="4"
        maxlength="500"
        class="w-full px-3 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring focus:ring-blue-200"
      ></textarea>
    </div>

    <div v-if="success" class="bg-green-100 text-green-700 p-3 rounded-md mb-4">
      ¡Gracias por tu mensaje! Nos pondremos en contacto contigo pronto.
    </div>

    <Button type="submit" :disabled="loading">
      {{ loading ? 'Enviando...' : 'Enviar Mensaje' }}
    </Button>
  </form>
</template>

<script setup>
import { ref, watch } from 'vue';
import Button from './Button.vue';
import Input from './Input.vue';

const formData = ref({
  name: '',
  email: '',
  message: '',
});

const errors = ref({
  name: '',
  email: '',
});

const success = ref(false);
const loading = ref(false);

const validateForm = () => {
  errors.value.name = formData.value.name.trim().length > 0 ? '' : 'El nombre es obligatorio.';
  errors.value.email = formData.value.email && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.value.email)
    ? ''
    : 'El correo electrónico no es válido.';

  return !errors.value.name && !errors.value.email;
};

// Validar en tiempo real para usabilidad
watch(() => formData.value.name, () => {
  if (formData.value.name.length > 0) errors.value.name = '';
});
watch(() => formData.value.email, () => {
  if (formData.value.email.length > 0 && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.value.email)) {
    errors.value.email = '';
  }
});

const handleSubmit = () => {
  if (!validateForm()) {
    return;
  }

  loading.value = true;
  success.value = false;

  // Lógica para enviar a la API
  // Aquí se enviaría a una API de backend segura
  // fetch('https://mi-api-segura.com/submit-lead', { ... });

  setTimeout(() => {
    loading.value = false;
    success.value = true;
    formData.value = { name: '', email: '', message: '' };
  }, 1500);
};
</script>