<script setup>
import { ref } from 'vue';
import SearchField from '@/components/SearchField.vue';

const places = ref([]);

const addPlace = ((data) => {
  places.value.push(data);
  console.log(places);
});

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
    <div>
      <SearchField @place-data="addPlace" />
    </div>
  </main>
</template>