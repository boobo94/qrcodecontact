<template>
  <div>
    <div class="row justify-center">
      <h1>Qr Code</h1>
    </div>

    <div class="row justify-center">
      <input
        type="text"
        v-model="scope.phoneNumber"
        @input="generateQrCode"
        class="phone-input"
      />
    </div>

    <div class="row justify-center">
      <button @click="generateQrCode" class="btn">Generate</button>
    </div>

    <div class="row justify-center">
      <div v-if="scope.qrcode" class="preview-container">
        <img :src="scope.qrcode" alt="qrcode preview" class="qrcode-preview" />
        <button @click="download" class="btn">Download</button>
      </div>
    </div>
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
