<script setup>
import Hero from '@/components/Hero.vue';
import HomeCrads from '@/components/HomeCards.vue';
import JobListnings from '@/components/JobListnings.vue';
</script>

<template>
    <Hero title="Test Title" subtitle="Test SubTitle" />
    <HomeCrads />
    <JobListnings :limit="3" :showButton="true" />
</template>