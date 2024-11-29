<script setup lang="ts">
interface Prop {
  title: string;
  selected: boolean;
}

defineProps<Prop>();
</script>

<template>
  <div class="plan" :class="{ 'active-plan': selected }">
    <div class="description">
      <span class="title"> {{ title }} </span>
    </div>
  </div>
</template>
