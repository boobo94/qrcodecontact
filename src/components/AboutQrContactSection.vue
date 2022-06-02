<template>
  <div>
    <div class="row justify-content-center">
      <h1>{{ t("page_title") }}</h1>
    </div>
  </div>
</template>

<script setup>
import { useI18n } from 'vue-i18n';

const { t } = useI18n({
  messages: {
    en: {

    },
    ro: {

    },
  },
});
</script>
