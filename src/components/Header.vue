<script setup>
import EcosystemIcon from './icons/IconEcosystem.vue';
import CommunityIcon from './icons/IconCommunity.vue';
import SupportIcon from './icons/IconSupport.vue';
</script>

<template>
  <header>
    <div class="flex w-full justify-between content-center">
      <img src="@/assets/brandLogo.png" width="200" />
      <div class="flex content-center justify-center self-center space-x-4">
        <div><EcosystemIcon /></div>
        <div><CommunityIcon /></div>
        <div><SupportIcon /></div>
      </div>
    </div>
  </header>
</template>

<style scoped>
header {
  padding: 1rem;
}
</style>
