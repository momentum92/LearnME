<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-container>
        <v-row align="center" justify="center">
          <v-img alt="Vuetify Logo"
                 class="shrink mr-2"
                 contain
                 src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
                 transition="scale-transition"
                 width="40"/>
        </v-row>
      </v-container>
    </v-app-bar>
    <v-main>
      <Layout/>
    </v-main>
  </v-app>
</template>

<script>
import Layout from './components/Layout';

export default {
  name: 'App',
  components: {
    Layout,
  },
  data: () => ({}),
};
</script>
