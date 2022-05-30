<template>
  <div class="language-selector">
    <h3>{{ t("language") }}</h3>
    <img :src="imageFlag" alt="" />
    <select v-model="locale">
      <option v-for="(lang, i) in langs" :key="`lang-${i}`" :value="lang">
        {{ lang }}
      </option>
    </select>
  </div>
</template>
<script setup>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

const langs = ['en', 'ro'];
const { locale, t } = useI18n();
const imageFlag = computed(() => `/images/${locale.value}-flag.png`);
</script>
<style>
.language-selector {
  display: flex;
  align-items: center;
}

img {
  margin-left: 10px;
  margin-right: 10px;
}

select {
  width: 75px;
  line-height: 49px;
  height: 38px;
  font-size: 22px;
  outline: 0;
}
</style>

<i18n>
{
  "en": {
    "language": "Language",
  },
  "ro": {
    "language": "Limba",
  }
}
</i18n>
