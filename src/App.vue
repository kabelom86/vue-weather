<script setup>
import SearchField from '@/components/SearchField.vue';
</script>

<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
      {{
        new Date().toLocaleDateString('en-za', {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
        })
      }}
    </div>
    <!-- Search Field -->
    <SearchField />
  </main>
</template>