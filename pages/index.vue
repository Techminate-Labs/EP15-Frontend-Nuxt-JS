<template>
  <div class="ma-5">
    <v-row >
      <!---calculator 1--->
      <v-col cols="12" sm="6" md="6">
        <Calculator1 />
      </v-col>
      <!---Calculator 2--->
      <v-col cols="12" sm="6" md="6">
        <Calculator2 />
      </v-col>
    </v-row>
    <!---Investmen plan--->
    <Investment />
    <!--End Investmen plan--->
  </div>
</template>

<script>
  import Calculator1 from '@/components/Calculator1.vue'
  import Calculator2 from '@/components/Calculator2.vue'
  import Investment from '~/components/Investment.vue'
  export default {  
    components: {
      Calculator1,
      Calculator2,
      Investment
    },
    data () {
      return {
      }
    },
  }
</script>

<style>

</style>