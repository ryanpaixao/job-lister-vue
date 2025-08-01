<script setup>
import { reactive, onMounted } from 'vue';
import { useRoute, RouterLink } from 'vue-router';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from 'axios';

import JobListing from '@/components/JobListing.vue';

const route = useRoute();
const jobId = route.params.id;

const state = reactive({
  job: {},
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get(`http://localhost:5000/jobs/${jobId}`);
    state.job = response.data;
  } catch (error) {
    console.log('Error fetching job: ', error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <!-- If NOT loading -->
  <JobListing v-if="!state.isLoading" :job="state.job" />

  <!-- If loading -->
  <div v-if="state.isLoading" class="text=center text-gray-500 py-6">
    <PulseLoader />
  </div>
</template>