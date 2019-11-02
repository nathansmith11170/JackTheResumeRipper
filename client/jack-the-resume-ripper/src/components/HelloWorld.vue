<template>
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
</template>

<script>
export default {
  name: 'HelloWorld'
};
</script>
