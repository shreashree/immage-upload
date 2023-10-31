<template>
  <div>
    <h1>Image Upload and Text Extraction</h1>
    <input type="file" @change="handleFileUpload" accept="image/*">  
    <button @click="populateTextArea">Show Extracted Text</button>
    <div v-if="imageUrl">
      <img :src="imageUrl" alt="Uploaded Image" style="max-width: 400px; margin-top:10px"><br><br>
    </div>
    <div>
      <textarea v-if="extractedText" v-model="extractedText" rows="10" cols="50" style="color:blue;" >extracted text</textarea>
      <p v-if="wordCount">Total Wrods : {{wordCount}} </p>
    </div>
    
  </div>
</template>

<script>
// tesseract is used to extract text from image
import Tesseract from 'tesseract.js';

export default {
  data() {
    return {
      selectedFile: null,
      imageUrl: '',
      extractedText: '',
      wordCount:0,
    };
  },
  methods: {
    handleFileUpload(event) {
      this.selectedFile = event.target.files[0];
      this.imageUrl = URL.createObjectURL(this.selectedFile);
    },
    populateTextArea() {
      if (this.selectedFile) {
        Tesseract.recognize(this.selectedFile, 'eng', { logger: info => console.log(info) })
          .then(({ data: { text } }) => {
            this.extractedText = text;
            this.wordCount = text.split(/\s+/).filter(word => word.trim() !== '').length;
          })
          .catch(error => {
            console.error('Error extracting text:', error);
          });
      }
    },
  },
};
</script>
<style>
p{
  color: blue;
}

</style>
