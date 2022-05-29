<template>
  <div>
    <h1>Qr Code</h1>
    <input type="text" v-model="scope.phoneNumber" @input="generateQrCode" />
    <button @click="generateQrCode">Generate</button>
    <button @click="download">Download</button>

    <img v-if="scope.qrcode" :src="scope.qrcode" alt="" />
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import QRCode from 'qrcode';

const scope = reactive({
  phoneNumber: '',
  qrcode: null,
});

async function generateQrCode() {
  scope.qrcode = await QRCode.toDataURL(scope.phoneNumber);
}

function download() {
  const fileLink = document.createElement('a');
  fileLink.href = scope.qrcode;
  fileLink.setAttribute('download', scope.phoneNumber);
  fileLink.setAttribute('target', '_blank');
  document.body.appendChild(fileLink);
  fileLink.click();
  fileLink.remove();
}
</script>

<style lang="sass" scoped>
</style>
