<template>
  <div class="home">
    <QrGenerator />
  </div>
</template>

<script setup>
import QrGenerator from '@/components/QrGenerator.vue';
</script>

<style lang="scss">
.container {
  display: flex;
  justify-content: space-around;
}

.row {
  display: flex;
  flex-wrap: nowrap;

  .column {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
  }
}

.justify-content-center {
  justify-content: center;
}

@media all and (max-width: 540px) {
  .row {
    flex-direction: column;
  }
}
</style>
