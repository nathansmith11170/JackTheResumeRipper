<template>
  <v-app>
    <v-app-bar app  color="error">
      <v-toolbar-title class="headline text-uppercase">
        <span>Jack </span>
        <span class="font-weight-light">The Resume Ripper</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text
        href="https://github.com/nathansmith11170/JackTheResumeRipper/graphs/contributors"
        target="_blank"
      >
        <span class="mr-2">Creators</span>
      </v-btn>
    </v-app-bar>

    <v-content>
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
            >
              <v-card-text>
                <p class="display-1 text--primary">
                  API Response
                </p>
                <p>Nothing yet</p>
              </v-card-text>
            </v-card>
          </v-flex>
          <v-flex mb-4>
            <v-btn outlined color="error" @click="uploadButtonClicked">Upload a Resume</v-btn>
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

export default {
  name: 'App',
  data: () => ({
      info: null,
      loading: true,
      errored: false,
      snackbarSuccess: false
  }),
  methods: {
    uploadButtonClicked () { // TODO: fix
      console.log('clicked')
      axios
      .get('https://jsonplaceholder.typicode.com/todos/1')
      .then(response => {
        this.info = response.data
        console.log(this.info)
        this.snackbarSuccess = true
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
    }
  }
};
</script>
