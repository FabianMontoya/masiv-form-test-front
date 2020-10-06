<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h2>MASIV TEST</h2>
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/FabianMontoya/masiv-form-test-front"
        target="_blank"
        text
      >
        <span class="mr-2">Ver en GitHub</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <consult-comic/>
    </v-main>
    
    <v-footer id="dashboard-footer" app padless inset elevation="4" :color="!$vuetify.theme.dark ? 'white' : ''">
      <v-container>
        <v-row align="center" no-gutters>
          <v-col cols="12" md="auto">
          <div class="text--secondary px-4 text-center">&copy; {{ new Date().getFullYear() }}</div>
          </v-col>
          <v-spacer class="hidden-sm-and-down" />
          <v-col cols="12" md="auto">
            <div class="font-weight-light text-center">
              Made with
              <v-icon size="18">
                mdi-heart
              </v-icon>
              by Fabian Montoya.
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-footer>
  </v-app>
</template>

<script>
import ConsultComic from './components/ConsultComic';

export default {
  name: 'App',

  components: {
    'consult-comic': ConsultComic,
  },

  data: () => ({
    //
  }),
};
</script>
