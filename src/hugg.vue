<template>
    <div>
        <div>加载了</div>
      <input v-model="inputText" @input="generateImage" placeholder="Enter text to generate image">
      <img :src="imageUrl" v-if="imageUrl" alt="Generated Image">
    </div>
  </template>
  
  <script>
  import { HfInference } from '@huggingface/inference';
  
  export default {
    data() {
      return {
        inputText: '',
        imageUrl: null,
        inference: new HfInference("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")
      };
    },
    methods: {
      async generateImage() {
        if (this.inputText.trim() === '') {
          this.imageUrl = null;
          return;
        }
        const response = await this.inference.textToImage({
          model: 'stabilityai/stable-diffusion-2',
          inputs: this.inputText
        });
        this.imageUrl = URL.createObjectURL(response.data);
      }
    }
  }
  </script>
  