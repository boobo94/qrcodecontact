<template>
  <div class="share-links">
    <button class="close" @click="$emit('onClose')">X</button>

    <div class="container">
      <div class="row">
        <div class="column left"><img :src="socialImage" /></div>
        <div class="column">
          <div>
            <h2>{{ t("share_title") }}</h2>
            <p>{{ t("share_description") }}</p>
            <div class="social-images">
              <a
                class="icon"
                :href="`https://facebook.com/sharer.php?u=${currentUrl}`"
                rel="nofollow"
                target="_blank"
                ><img :src="twitterImage"
              /></a>
              <a
                class="icon"
                :href="`https://twitter.com/intent/tweet?text=${currentUrl}`"
                rel="nofollow"
                target="_blank"
                ><img :src="facebookImage"
              /></a>

              <a
                class="icon"
                :href="`http://www.linkedin.com/shareArticle?url=${currentUrl}`"
                rel="nofollow"
                target="_blank"
                ><img :src="linkedinImage"
              /></a>

              <a
                class="icon copy-icon"
                rel="nofollow"
                target="_blank"
                @click="copyToClipboard"
                ><img :src="copyImage"
              /></a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
/* eslint-disable global-require */

import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

const { t } = useI18n({
  messages: {
    en: {
      share_title: 'Share QR Code Contact',
      share_description:
        'Select your favorite social network and share our QR tool with your friends, if you do not have these social networks copy the link and paste it into the one you use.',
    },
    ro: {
      share_title: 'Distribuie QR Code Contact',
      share_description:
        'Selectați rețeaua socială preferată și partajați instrumentul nostru QR cu prietenii dvs., dacă nu aveți aceste rețele sociale, copiați linkul și inserați-l în cel pe care îl utilizați.',
    },
  },
});

const currentUrl = window.location.href;

const twitterImage = computed(() => require('@/assets/images/twitter.png'));
const facebookImage = computed(() => require('@/assets/images/facebook.png'));
const linkedinImage = computed(() => require('@/assets/images/linkedin.png'));
const copyImage = computed(() => require('@/assets/images/copy.png'));
const socialImage = computed(() => require('@/assets/images/share-image.png'));

function copyToClipboard() {
  navigator.clipboard.writeText(currentUrl);
}
</script>

<style lang="scss" scoped>
.share-links {
  position: absolute;
  width: 50%;
  margin: auto;
  padding: 20px;
  height: fit-content;
  z-index: 1;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;

  color: #81c784;

  .close {
    position: absolute;
    right: 10px;
    top: 10px;
  }

  a {
    margin-right: 5px;
  }

  .row {
    flex: 1;
  }

  .column.left {
    img {
      width: 250px;
      height: 250px;
    }
  }

  .social-images {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin-top: 50px;

    .icon {
      &.copy-icon {
        cursor: pointer;
      }

      img {
        width: 50px;
        height: 50px;
      }
    }
  }
}

@media all and (max-width: 540px) {
  .share-links {
    width: 80%;
  }
}
</style>
