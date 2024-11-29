<script setup lang="ts">
import { ref } from 'vue';
import CoffeePlan from '@/components/CoffeePlan.vue';
import { type ICoffeePlan } from '@/types';

const plans = ref<ICoffeePlan[]>([
  { title: 'The Single' },
  { title: 'The Curious' },
  { title: 'The Addict' },
  { title: 'The Hacker' },
]);
const selectedPlan = ref('');

const handleClickCoffeePlan = (title: string) => {
  selectedPlan.value = title;
};
</script>

<template>
  <div class="plans">
    <CoffeePlan
      v-for="(plan, index) in plans"
      :key="`plan-${index}`"
      :title="plan.title"
      :selected="selectedPlan === plan.title"
      @click="handleClickCoffeePlan(plan.title)"
    />
  </div>
</template>
