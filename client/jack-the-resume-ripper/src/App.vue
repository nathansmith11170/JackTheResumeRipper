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
            <v-btn outlined color="error">Upload a Resume</v-btn>
          </v-flex>
        </v-layout>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

export default {
  name: 'App',
  data: () => ({
    //
  }),
};
</script>
