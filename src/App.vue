<template>
  <div id="app">
    <amplify-authenticator>
      <amplify-sign-in
        header-text="Image Upload & Rekognition Demo"
        slot="sign-in"
      ></amplify-sign-in>
      <div>
        <img alt="Vue logo" src="./assets/logo.png" />
        <div v-if="img === ''">
          <h3>Upload Image</h3>
          <input type="file" accept="image/jpeg" @change="putS3Image($event)" />
        </div>
        <div style="margin:10px" v-if="result !== ''">
          <strong>Objects found:</strong> {{ result.join(', ') }}
        </div>
        <div v-if="img !== ''">
          <img v-bind:src="img" />
        </div>
        <div id="amplify-signout">
          <amplify-sign-out></amplify-sign-out>
        </div>
      </div>
    </amplify-authenticator>
  </div>
</template>
<script>
import Storage from '@aws-amplify/storage';
import API, { graphqlOperation } from '@aws-amplify/api';
import { speakTranslatedImageText } from './graphql/queries';
export default {
  name: 'App',
  data() {
    return {
      result: '',
      img: '',
      file: '',
    };
  },
  methods: {
    handleFileUpload() {
      this.file = this.$refs.file.files[0];
    },
    putS3Image(event) {
      const file = event.target.files[0];
      Storage.put(file.name, file)
        .then(async (result) => {
          this.result = await this.speakTranslatedImageTextOP(result.key);
          this.img = await Storage.get(result.key);
        })
        .catch((err) => console.log(err));
    },
    async speakTranslatedImageTextOP(key) {
      const inputObj = {
        identifyLabels: { key },
      };
      const response = await API.graphql(
        graphqlOperation(speakTranslatedImageText, { input: inputObj })
      );
      return response.data.speakTranslatedImageText;
    },
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#amplify-signout {
  max-width: 200px;
  margin: 20px auto;
}
</style>
