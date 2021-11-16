<template>
  <div>
    <!---investmen plan---->
      <v-row justify="space-around">
         <v-col cols="12" sm="10" md="8">
            <v-sheet elevation="10" class="py-4 px-1 ma-5" align="center" justify="center">
              <h1>Want to make an Investment plan? </h1>
              <span>Select A Trend </span>
            </v-sheet>  
        </v-col>
      </v-row>
    <!---investmen plan---->
    <!---button --->
    <v-container>
     <v-row class="pb-5" align="center" justify="center">
        <v-btn class="pa-4" tile>
          <NuxtLink to="/uptrend"><span class="white--text">Up Trend</span></NuxtLink>
        </v-btn>
        <v-btn class="pa-4 ml-5" tile>
          <NuxtLink to="/downtrend"><span class="white--text">Down Trend</span></NuxtLink>
        </v-btn>
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