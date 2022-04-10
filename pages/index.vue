<script setup lang="ts">
import Form from '~~/components/Form.vue';
definePageMeta({
  middleware: 'auth'
});
</script>
<template>
<NuxtLayout name="custom">
  <div class="w-full min-h-screen gridBox bg-black">
    <Form/>
  </div>
</NuxtLayout>
</template>