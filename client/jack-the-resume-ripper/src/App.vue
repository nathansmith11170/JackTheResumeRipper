<template>
  <v-app>
    <v-app-bar app  color="error">
      <v-toolbar-title class="headline text-uppercase">
        <span>Jack </span>
        <span class="font-weight-light">The Resume Ripper</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn text @click="overlay = !overlay">
        About This Project
      </v-btn>
      <v-btn
        text
        href="https://github.com/nathansmith11170/JackTheResumeRipper/graphs/contributors"
        target="_blank"
      >
        <span class="mr-2">Creators</span>
      </v-btn>
    </v-app-bar>

    <v-content>
    <v-dialog
      v-model="overlay"
      max-width="700"
    >
      <v-card>
        <v-card-title class="headline">About Jack The Resume Ripper</v-card-title>
        <v-card-text>
          <p>Jeffery Morhous</p>
          <p>Nathan Smith</p>
          <p>Jonathan Soldan</p>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
            <v-btn
              color="green darken-1"
              text
              @click="overlay = false"
            >
              Disagree
            </v-btn>

            <v-btn
              color="green darken-1"
              text
              @click="overlay = false"
            >
             Agree
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-container>
        <v-layout
          text-center
          wrap
        > 
          <v-flex
            xs12
            mb-5
          >
            <v-card
            class="mx-auto"
            max-width="344"
            v-if="info!=null"
            >
              <v-card-text>
                <p class="display-1 text--primary">
                  API Response
                </p>
                <div>{{this.info}}</div>
              </v-card-text>
            </v-card>
            <v-card
            class="mx-auto pa-9"
            max-width="444"
            v-if="info==null"
            >
              <v-card-text>
                <p class="display-1 text--primary">
                  Enter search keywords
                </p>
                <div>{{this.info}}</div>
              </v-card-text>
              <v-text-field v-model="keywords" label="Keywords"></v-text-field>
              <v-file-input v-model="filePointer" label="File input"></v-file-input>
            </v-card>
          </v-flex>
          <v-flex mb-4>
              <v-btn
              raised color="error"
              @click="uploadButtonClicked"
              :disabled="ripButtonDisabled">Rip it!</v-btn> 
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
    <v-snackbar
      v-model="snackbarSuccess"
    >
      Resume succesfully parsed!
      <v-btn
        color="error"
        text
        @click="snackbarSuccess = false"
      >
        Close
      </v-btn>
    </v-snackbar>
    <v-snackbar
      v-model="snackbarError"
    >
      Resume parser had an error, our bad!
      <v-btn
        color="error"
        text
        @click="snackbarError = false"
      >
        Close
      </v-btn>
    </v-snackbar>
  </v-app>
</template>

<script>
import axios from 'axios';
import enviroment from './enviroment';

export default {
  name: 'App',
  data: () => ({
      info: null,
      loading: true,
      errored: false,
      snackbarSuccess: false,
      snackbarError: false,
      keywords: "",
      filePointer: null,
      overlay: false
  }),
  methods: {
    async uploadButtonClicked() { // TODO: fix
      const toBase64 = file => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result);
        reader.onerror = error => reject(error);
      });

      var base64Image = await toBase64(this.filePointer);
      //console.log(base64Image)
      //var imageString = base64Image.substring(base64Image.indexOf(',')+1)
      await axios
      .post(enviroment.VUE_APP_RESUME_ANALYSIS_ENDPOINT, {
          image: String(base64Image),
          keywords: this.keywords
        })
      .then(response => {
        this.info = response.data.image
        console.log(this.info)
        this.snackbarSuccess = true
      })
      .catch(error => {
        console.log(error)
        this.errored = true
        this.snackbarError = true
      })
      .finally(() => this.loading = false)
    }
  },
  computed: {
    ripButtonDisabled: function () {
      return (this.filePointer == null) ? true : false;
    }
  }
};
</script>
