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
        @click="overlay2 = !overlay2"
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
          <p>This is a web application that intends to use Microsoft Azure to parse a resume file, searching for indicators of desirable characteristics and URL's. In its current release, the application only returns the occurrence of provided keywords in the given file.</p>
          <p>To use the app, enter keywords separated by commas, and select a resume from your system. Then click 'Rip it!' and let Jack do his thing. The results will be displayed promptly.</p>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
            <v-btn
              color="red darken-1"
              text
              @click="overlay = false"
            >
             close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      <v-dialog
      v-model="overlay2"
      max-width="700"
      >
        <v-card>
          <v-card-title class="headline">About Jack's Parents'</v-card-title>
          <v-card-text>
            <div>
               <v-row justify="space-around">
                <h2 color="white">Jeffery Morhous</h2>
   
                <a href="https://www.linkedin.com/in/jeffery-morhous/"><v-icon>mdi-linkedin</v-icon></a>

                <a href="https://github.com/JeffMorhous"><v-icon>mdi-github-circle</v-icon></a>

                <a href="https://twitter.com/thejeffmor"><v-icon>mdi-twitter</v-icon></a>

                <a href="https://medium.com/@jeffmorhous/latest"><v-icon>mdi-medium</v-icon></a>
              </v-row>
              <br>
              <p>Jeffery built and expanded the client-side of Jack the Resume Ripper. 
                He guided the Vue.js and design aspects, as well as project architecture.</p>

            </div>
            <div>
              <h2>Nathan Smith</h2>
            </div>
             <div>
              <h2>Jonathan Soldan</h2>
            </div>
          </v-card-text>
          <v-card-actions>
            <v-btn
            color="error"
            outlined
            href="https://github.com/nathansmith11170/JackTheResumeRipper/graphs/contributors"
            target="_blank">
            Commit History</v-btn>
            <v-spacer></v-spacer>
              <v-btn
                color="red darken-1"
                text
                @click="overlay2 = false"
                outlined
              >
              close
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
                <p class="display-1 text--primary">Keyword String</p>
                <div>{{this.keywords}}</div>
                <p class="display-1 text--primary">
                  API Response
                </p>
                <div>{{this.info}}</div>
              </v-card-text>
            </v-card>
            <v-card
            class="mx-auto pa-9"
            v-if="info==null"
            >
              <v-card-text>
                <p class="display-1 text--primary">
                  Jack The Resume Ripper
                </p>
                <div>{{this.info}}</div>
              </v-card-text>
              <v-text-field v-model="keywords" label="Keywords, separated by commas"></v-text-field>
              <v-file-input v-model="filePointer" label="File input"></v-file-input>
            </v-card>
          </v-flex>
          <v-flex mb-4>
              <v-btn
              raised color="error"
              :loading="loading"
              @click="uploadButtonClicked"
              :disabled="ripButtonDisabled"
              block
              v-if="info==null">Rip it!</v-btn>
              <v-btn
              raised color="error"
              @click="resetApp"
              block
              v-if="info!=null">Rip again?</v-btn>
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
      loading: false,
      errored: false,
      snackbarSuccess: false,
      snackbarError: false,
      keywords: "",
      filePointer: null,
      overlay: false,
      overlay2: false
  }),
  methods: {
    async uploadButtonClicked() { // TODO: fix
      const toBase64 = file => new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result);
        reader.onerror = error => reject(error);
      });
      this.loading = true;
      var base64Image = await toBase64(this.filePointer);
      //console.log(base64Image)
      //var imageString = base64Image.substring(base64Image.indexOf(',')+1)
      await axios
      .post(enviroment.VUE_APP_RESUME_ANALYSIS_ENDPOINT, {
          image: String(base64Image),
          keywords: this.keywords
        })
      .then(response => {
        console.log(response.data.Error)
        if(response.data.Error != null) {
          this.errored = true
          this.snackbarError = true
        } else {
          this.info = response.data.values
          console.log(this.info)
          this.snackbarSuccess = true
        }
      })
      .catch(error => {
        console.log(error)
        this.errored = true
        this.snackbarError = true
      })
      .finally(() => this.loading = false)
    },
    resetApp() {
      this.info = null
      this.loading = false
      this.errored = false
      this.snackbarSuccess = false
      this.snackbarError = false
      this.keywords = ""
      this.filePointer = null
      this.overlay = false
      this.overlay2 = false
    }
  },
  computed: {
    ripButtonDisabled: function () {
      return (this.filePointer == null) ? true : false;
    }
  }
};
</script>
