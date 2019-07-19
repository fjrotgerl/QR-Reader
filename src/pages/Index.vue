<template>
  <q-page>
    <!-- Body -->
    <div class="flex flex-center q-layout-padding">
      <p class="text-h6 text-weight-regular w-80">Selecciona el archivo: </p>
      <q-uploader
        url="http://localhost:4444/upload"
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

  export default {
    name: 'PageIndex',
    data() {
      return {
        // Variables
        isUploadButtonDisabled: true,
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
        dropbox: () => {
          // this.$axios.post("https://content.dropboxapi.com/2/files/download")
          // //   , {
          // //   headers: {
          // //     "Authorization": "Bearer bkAs6K8C15AAAAAAAAAAM_BSwzMnqpFT4SE8Kpr2xAFsY0nmY0jrxVkIgtjWrTzK",
          // //     "Dropbox-API-Arg": "{\"path\": \"/Homework/math/Prime_Numbers.txt\"}"
          // //   }
          // // })
          //   .then(response => console.log(response))
          //   .catch(error => console.error(error))
          this.$axios.post("https://api.dropboxapi.com/2/users/get_account", {
            data: "{\"account_id\": \"dbid:AAH4f99T0taONIb-OurWxbNQ6ywGRopQngc\"}"
            },
            {
            headers: {
              "Authorization": "Bearer bkAs6K8C15AAAAAAAAAANivquWVMzvaiaLY3w4tNaMHsLA3eFcpuPMN5AGI8eDjy",
              "Content-Type": "application/json"
            }
          })
            .then(response => console.log(response.json()))
            .catch(error => console.error(error))
        }
      }
    }
  }
</script>
