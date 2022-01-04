<template>
  <div class="container">
    <h1>Upload a file to read its contents!</h1>
    <input
      type="file"
      id="file"
      accept="image/png, image/jpeg"
      @change="preloadFile"
    />
    <button v-if="file" class="loadButton" @click="readImage">
      Read from image!
    </button>

    <div class="loading" v-if="progress > 0">
      Progress: {{progress*100}}%
    </div>

    <div class="imageContent" v-if="imageText">
      <h2>Here is your text:</h2>
      {{imageText}}
    </div>
    
  </div>
</template>

<script>
import Tesseract from "tesseract.js";

export default {
  name: "Main",

  data() {
    return {
      file: undefined,
      progress: 0,
      imageText: ''
    };
  },

  methods: {
    preloadFile(event) {
      const file = event.target.files[0];
      console.log(file);
      this.file = file;
    },

    readImage() {
      console.log(this.file);

      Tesseract.recognize(this.file, "eng", {
        logger: (m) => {
          this.progress = m.progress
        },
      }).then(({ data: { text } }) => {
        this.imageText = text
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 800px;
}

.loadButton{
  margin: 35px 0;
}
</style>
