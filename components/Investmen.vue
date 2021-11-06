<template>
  <div>
    <!---investmen plan---->
      <v-row justify="space-around">
         <v-col cols="12" sm="10" md="8">
            <v-sheet elevation="10" class="py-4 px-1 ma-5" align="center" justify="center">
              <h1>Want to make an Investment plan? </h1>
              <span>Selecte the trend </span>
            </v-sheet>  
        </v-col>
      </v-row>
    <!---investmen plan---->
    <!---button --->
    <v-container>
     <v-row class="ma-5" align="center" justify="center">
        <v-col cols="12" sm="6" md="4">
          <v-card class="pa-4" outlined tile align="center" justify="center">
              <NuxtLink to="/Uptend"><span class="white--text">Up Trend</span></NuxtLink>
          </v-card>
        </v-col>
        <v-col cols="12" sm="6" md="4">
          <v-card class="pa-4" outlined tile align="center" justify="center">
              <NuxtLink to="/Downtend"><span class="white--text">Down Trend</span></NuxtLink>
          </v-card>
        </v-col>
     </v-row>
    </v-container>
    <!---end button---->
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>