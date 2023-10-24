<template>
  <div>
    <h1>Image Upload and Text Extraction</h1>
    <input type="file" @change="handleFileUpload" accept="image/*">
    <button @click="extractText">Upload and Extract Text</button>
    <div v-if="imageUrl">
      <img :src="imageUrl" alt="Uploaded Image" style="max-width: 400px;">
    </div>
    <div v-if="extractedText">
      <p>{{ extractedText }}</p>
    </div>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js';

export default {
  data() {
    return {
      selectedFile: null,
      imageUrl: '',
      extractedText: '',
    };
  },
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
      this.imageUrl = URL.createObjectURL(this.selectedFile);
    },
    extractText() {
      if (this.selectedFile) {
        Tesseract.recognize(this.selectedFile, 'eng', { logger: info => console.log(info) })
          .then(({ data: { text } }) => {
            this.extractedText = text;
          })
          .catch(error => {
            console.error('Error extracting text:', error);
          });
      }
    },
  },
};
</script>
