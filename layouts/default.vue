<template>
  <v-app dark>
    <v-app-bar
      fixed
      app
    >
      <NuxtLink to="/" class="mr-2">
        <v-img alt="logo" height="45" width="45" src="https://cdn-icons-png.flaticon.com/512/564/564429.png"></v-img>
      </NuxtLink>
      <v-spacer></v-spacer>
      <NuxtLink to="/" class="ma-5">
        <span class="grey--text">Home</span>
      </NuxtLink>
    </v-app-bar>
    <v-main>
      <Nuxt />
    </v-main>
  <v-footer
    :absolute="!fixed"
    app
  >
    <v-row
      justify="space-between"
      no-gutters
    >
      <strong>Developed By : Techminate</strong>
      <strong>Copyright@Techminate {{ new Date().getFullYear() }} </strong>
    </v-row>
  </v-footer>

  </v-app>
</template>

<script>
export default {
  data () {
    return {
      fixed: false,
    }
  }
}
</script>
<style>
  a {
    text-decoration: none;
  }
</style>