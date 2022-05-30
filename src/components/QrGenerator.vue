<template>
  <div>
    <div class="row justify-center">
      <h1>{{ t("qr-code") }}</h1>
    </div>

     <div class="row justify-center">
      <LanguageSelector/>
    </div>

    <div class="row justify-center">
      <input
        type="text"
        v-model="scope.phoneNumber"
        @input="addQrCodeToCanvas"
        class="phone-input"
      />
    </div>

    <div class="row justify-center">
      <button @click="addQrCodeToCanvas" class="btn">
        {{ t("generate") }}
      </button>
    </div>

    <div class="row justify-center">
      <div v-show="scope.qrcode" class="preview-container">
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
</template>

<script setup>
import LanguageSelector from '@/components/LanguageSelector.vue';
import { reactive, ref } from 'vue';
import QRCode from 'qrcode';
import { useI18n } from 'vue-i18n';

const scope = reactive({
  phoneNumber: '',
  qrcode: null,
  ctx: null,
});
const { t } = useI18n();
const canvasResult = ref(null);

async function addQrCodeToCanvas() {
  scope.qrcode = await QRCode.toDataURL(scope.phoneNumber);

  const ctx = canvasResult.value.getContext('2d');

  // set the background for canvas
  ctx.fillStyle = '#fff';
  ctx.fillRect(0, 0, canvasResult.value.width, canvasResult.value.height);
  ctx.fillStyle = '#000';

  // create new image and add it to canvas
  const img = new Image(200, 200);
  img.src = scope.qrcode;
  img.onload = () => {
    ctx.drawImage(
      img,
      (canvasResult.value.width - img.width) / 2,
      0,
      img.width,
      img.height,
    );
  };

  // add cta to canvas
  ctx.font = '18px Arial';
  const ctaText = ctx.measureText('Suna-ma');
  ctx.fillText(
    t('cta'),
    (canvasResult.value.width - ctaText.width) / 2,
    img.height + 18,
  );

  const phoneNumberText = ctx.measureText(scope.phoneNumber);
  ctx.fillText(
    scope.phoneNumber,
    (canvasResult.value.width - phoneNumberText.width) / 2,
    img.height + 40,
  );
}

function download() {
  const fileLink = document.createElement('a');
  fileLink.href = canvasResult.value.toDataURL();
  fileLink.setAttribute('download', scope.phoneNumber);
  fileLink.setAttribute('target', '_blank');
  document.body.appendChild(fileLink);
  fileLink.click();
  fileLink.remove();
}
</script>

<style lang="scss" scoped>
.row {
  display: flex;
  flex-wrap: nowrap;
  padding: 1rem;
}

.justify-center {
  justify-content: center;
}

.phone-input {
  border: 1px solid #d7d9dd;
  border-radius: 36px;
  opacity: 1;
  padding: 5px;
  line-height: 1.5;
  height: 30px;
  width: 250px;
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
}
</style>

<i18n>
{
  "en": {
    "qr-code": "QR Code Parking",
    "phone-number": "Phone number",
    "generate": "Generate",
    "download": "Download",
    "cta": "Call me"
  },
  "ro": {
    "qr-code": "QR Code Parking",
    "phone-number": "Număr de telefon",
    "generate": "Generează",
    "download": "Descarcă",
    "cta": "Sună-mă"
  },
}
</i18n>
