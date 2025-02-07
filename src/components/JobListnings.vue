<script setup>
import { RouterLink } from 'vue-router';
import JobListning from './JobListning.vue';
import { reactive, defineProps, onMounted } from 'vue';
import axios from 'axios';
import PauseLoader from 'vue-spinner/src/PulseLoader.vue';

defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    }
})

const state = reactive({
    jobs: [],
    isFileLoading: true,
    error: null
});

onMounted(async () => {
    try {
        const response = await axios.get('/api/jobs');
        state.jobs = response.data;
        console.log('Fetched jobs:', state.jobs); // Add this for debugging
    } catch (error) {
        console.error("Error fetching jobs:", error);
        state.error = 'Failed to load jobs';
    } finally {
        state.isFileLoading = false;
    }
});
</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="conatiner-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>
            <!--Show Loading Spinner While Loading is True-->
            <div v-if="state.isFileLoading" class="text-center text-gray-500 py-6">
                <PauseLoader color="#38a169" />
            </div>

            <!--Show error if any-->
            <div v-else-if="state.error" class="text-center text-red-500 py-6">
                {{ state.error }}
            </div>

            <!--Show job listings when done loading-->
            <div v-else-if="state.jobs.length > 0" class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListning v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />
            </div>

            <!--Show message if no jobs-->
            <div v-else class="text-center text-gray-500 py-6">
                No jobs found
            </div>
        </div>
    </section>

    <section v-if="showButton"  class="m-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</RouterLink
      >
    </section>
</template>