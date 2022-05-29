<template>
  <div class="home">
    <QrGenerator/>
  </div>
</template>

<script>
// @ is an alias to /src
import QrGenerator from '@/components/QrGenerator.vue';

export default {
  name: 'Home',
  components: {
    QrGenerator,
  },
};
</script>
