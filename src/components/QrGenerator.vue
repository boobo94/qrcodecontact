<template>
  <div>
    <SharePopup
      v-if="scope.showSharePopup"
      @onClose="scope.showSharePopup = !scope.showSharePopup"
    />

    <div class="container">
      <div class="row">
        <div class="column">
          <div class="row">
            <h2>{{ t("title_step1") }}</h2>
          </div>
          <div class="row">
            <LanguageSelector />
          </div>

          <div class="row">
            <div class="form-field">
              <label for="line2">{{ t("line2_label") }}</label>
              <input
                type="text"
                name="line2"
                placeholder="ex: +1 (555) 555-5555"
                v-model="scope.line2"
                @input="addQrCodeToCanvas"
              />
            </div>
          </div>

          <div class="row">
            <div class="form-field">
              <label for="line1">{{ t("line1_label") }}</label>
              <input
                type="text"
                name="line1"
                :placeholder="t('line1_placeholder')"
                v-model="scope.line1"
                @input="addQrCodeToCanvas"
              />
            </div>
          </div>
        </div>

        <div
          class="column hide"
          :class="{
            show: scope.hasCanvas,
          }"
        >
          <div class="row">
            <h2>{{ t("title_step2") }}</h2>
          </div>
          <div class="row">
            <div class="preview-container">
              <canvas
                ref="canvasResult"
                class="canvas-draw"
                width="250"
                height="250"
              ></canvas>
              <button @click="download" class="btn">{{ t("download") }}</button>
            </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script setup>
import LanguageSelector from '@/components/LanguageSelector.vue';
import { reactive, ref } from 'vue';
import QRCode from 'qrcode';
import { useI18n } from 'vue-i18n';
import SharePopup from './SharePopup.vue';

const scope = reactive({
  line1: '',
  line2: '',
  hasCanvas: false,
  showSharePopup: false,
});
const { t } = useI18n({
  messages: {
    en: {
      download: 'Download',
      line1_label: 'Call to action',
      line1_placeholder: 'Call me',
      line2_label: 'Fill the phone number or email',
      title_step1: 'Step 1: Set the content',
      title_step2: 'Step 2: Preview',
    },
    ro: {
      download: 'Descarcă',
      line1_label: 'Call to action',
      line1_placeholder: 'Sună-mă',
      line2_label: 'Introdu numărul de telefon sau adresa de email',
      title_step1: 'Pasul 1: Alege conținutul',
      title_step2: 'Pasul 2: Previzualizare',
    },
  },
});
const canvasResult = ref(null);

function detectContacType(text) {
  const emailRegexp = /^[a-zA-Z0-9.!#$%&'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$/;
  const phoneRegexp = /^(\+[1-9]{1})?[0-9]{3,14}$/;

  if (emailRegexp.test(text)) {
    return `mailto:${text}`;
  } if (phoneRegexp.test(text)) {
    return `tel:${text}`;
  }

  return text;
}

async function addQrCodeToCanvas() {
  scope.hasCanvas = !!(scope.line1 || scope.line2);

  const ctx = canvasResult.value.getContext('2d');

  // set the background for canvas
  ctx.fillStyle = '#fff';
  ctx.fillRect(0, 0, canvasResult.value.width, canvasResult.value.height);
  ctx.fillStyle = '#000';

  // create new image and add it to canvas
  const img = new Image(200, 200);
  if (scope.line2) {
    img.src = await QRCode.toDataURL(detectContacType(scope.line2));
    img.onload = () => {
      ctx.drawImage(
        img,
        (canvasResult.value.width - img.width) / 2,
        0,
        img.width,
        img.height,
      );

      // add sponsor text to canvas
      const sponsor = 'Software by cmevo.com/parkqr';
      ctx.font = '12px Arial';
      const sponsorText = ctx.measureText(sponsor);
      ctx.fillText(
        sponsor,
        (canvasResult.value.width - sponsorText.width) / 2,
        20,
      );
    };
  }

  ctx.font = '18px Arial';

  // add line1 to canvas
  const line1Text = ctx.measureText(scope.line1);
  ctx.fillText(
    scope.line1,
    (canvasResult.value.width - line1Text.width) / 2,
    img.height + 18,
  );

  // add line1 to canvas
  const line2Text = ctx.measureText(scope.line2);
  ctx.fillText(
    scope.line2,
    (canvasResult.value.width - line2Text.width) / 2,
    img.height + 40,
  );
}

function download() {
  const fileLink = document.createElement('a');
  fileLink.href = canvasResult.value.toDataURL();
  fileLink.setAttribute('download', detectContacType(scope.line2));
  fileLink.setAttribute('target', '_blank');
  document.body.appendChild(fileLink);
  fileLink.click();
  fileLink.remove();
  scope.showSharePopup = true;
}
</script>

<style lang="scss" scoped>
.form-field {
  display: flex;
  flex-direction: column;

  input {
    border: 1px solid #81c784;
    border-radius: 36px;
    opacity: 1;
    padding: 5px;
    line-height: 1.5;
    height: 30px;
    width: 250px;
  }
}

.preview-container {
  display: flex;
  flex-direction: column;

  .canvas-draw {
    margin-top: 10px;
    margin-bottom: 10px;
  }

  .qrcode-preview {
    width: 200px;
    height: 200px;
  }
}

.btn {
  background-color: #81c784;
  border: 1px solid #81c784;
  color: #fff;
  border-radius: 36px;
  padding: 10px 20px;
  cursor: pointer;
  min-width: 85px;
  font-weight: 500;
  letter-spacing: 0.0892857143em;
  flex-grow: 1;
}

.hide {
  visibility: hidden;
  opacity: 0;
  transition: visibility 0s, opacity 0.5s linear;
  position: absolute;
}

.show {
  visibility: visible;
  opacity: 1;
  position: relative;
}
</style>
