<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Name"
          class="mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="github_vuewer.png"
          width="200"
        />
      </div>
    </v-app-bar>

    <v-main>
      <GithubVuewer/>
    </v-main>
  </v-app>
</template>

<script>
import GithubVuewer from './components/GithubVuewer';

export default {
  name: 'App',

  components: {
    GithubVuewer,
  },

  data: () => ({
    //
  }),
};
</script>
