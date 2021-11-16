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
  
      <v-btn text>
        <span>Home</span>         
      </v-btn>
     <v-btn text>
        <span>About</span>         
      </v-btn>
      <v-btn text>
        <span>Contact</span>         
      </v-btn>
    </v-app-bar>
    <v-main>
        <Nuxt />
    </v-main>
    <v-footer
    padless
    :absolute="!fixed"
    app
  >
    <v-row
      justify="center"
      no-gutters
    >
      <v-btn
        v-for="link in links"
        :key="link"
        color="white"
        text
        rounded
        class="my-2"
      >
        {{ link }}
      </v-btn>
      <v-col
        class="py-4 text-center white--text"
        cols="12"
      >
        {{ new Date().getFullYear() }} — <strong>Techminate</strong>
      </v-col>
    </v-row>
  </v-footer>

  </v-app>
</template>

<script>
export default {
  data () {
    return {
      fixed: true,
      links: [
        'Home',
        'About Us',
        'Team',
        'Services',
        'Blog',
        'Contact Us',
      ],

    }
  }
}
</script>
<style>
  a {
    text-decoration: none;   
}
</style>