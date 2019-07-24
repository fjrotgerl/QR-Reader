<template>
  <q-page>
    <!-- Body -->
    <div class="flex flex-center q-layout-padding">
      <p class="text-h6 text-weight-regular w-80">Selecciona el archivo: </p>
      <q-uploader
        url="https://content.dropboxapi.com/2/files/upload"
        method="POST"
        :headers="uploadHeaders"
        ref="uploaderRef"
        @added="isUploadButtonDisabled = false"
        @removed="isUploadButtonDisabled = true"
        class="w-80"
      />
      <q-btn :disabled="isUploadButtonDisabled" @click="customUploadButton" label="Subir" class="bg-positive" style="width: 50%; margin-top: 40px;"></q-btn>
      <q-btn label="Generar" @click="generateQr"></q-btn>
      <q-btn label="Dropbox" @click="dropbox"></q-btn>

    </div>

    <!-- QR Code -->
    <div id="qrcode">
      <canvas id="canvas"></canvas>
    </div>
  </q-page>
</template>

<style>
</style>

<script>
  import qrcode from 'qrcode';
  import saveAs from 'file-saver';

  export default {
    name: 'PageIndex',
    data() {
      return {
        // Variables
        isUploadButtonDisabled: true,
        uploadHeaders: [
          {'Authorization': 'Bearer bkAs6K8C15AAAAAAAAAAOnboX3_8iGF_KFQ_ga5HskvIUDFByEJydoEJcHP_VP8b'},
          {'Dropbox-API-Arg': "{\"path\": \"/test.png\", \"mode\": \"add\",\"autorename\": true,\"mute\": false,\"strict_conflict\": false}"},
          {"Content-Type": "application/octet-stream"}
        ],
        // Functions
        customUploadButton: () => {
          this.$refs.uploaderRef.upload()
        },
        generateQr: () => {
          let canvas = document.getElementById('canvas');
          qrcode.toCanvas(canvas, 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAsAAAAKCAYAAABi8KSDAAAACXBIWXMAAA7DAAAOwwHHb6hkAAAAIElEQVQoU2PYJCz3Hx82EGWB41HFdFIcosL5nzjM+R8At/W6qT61VowAAAAASUVORK5CYII=', function (error) {
            if (error) console.error(error);
            console.log('success!');
          })
        },
        dropbox: async () => {
          await fetch('https://content.dropboxapi.com/2/files/download', {
            method: 'POST',
            headers: {
              'Authorization': 'Bearer bkAs6K8C15AAAAAAAAAAM_BSwzMnqpFT4SE8Kpr2xAFsY0nmY0jrxVkIgtjWrTzK',
              'Dropbox-API-Arg': "{\"path\": \"/101015.jpg\"}"
            }
          })
            .then(response => response.blob())
            .then(blob => saveAs(blob, "img.png"))
            .catch(error => console.error(error));
        }
      }
    }
  }
</script>
