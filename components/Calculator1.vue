<template>
  <div>
    <!---calculator--->
    <v-card class="pa-4" outlined tile >
        <v-card-title align="center" justify="center">Calculator 1</v-card-title>
        <v-card-text>
           <v-form v-model="valid" lazy-validation ref="form" @submit.prevent="calculate()" enctype="multipart/form-data">
                <v-text-field v-model="amountofTNBC" label="Amount of TNBC" placeholder="6" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-text-field v-model="rate" label="Rate" placeholder="100" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-card-actions>
                    <v-list-item class="grow">
                    <v-btn type="submit" :disabled="!valid" @click="validate" color="primary" tile>Calculate</v-btn>
                    <v-row align="center" justify="end">     
                        <span>Result: 100</span>
                    </v-row>
                    </v-list-item>
                </v-card-actions>
            </v-form>
        </v-card-text>
    </v-card>
    <!---end calculator--->
  </div>
</template>

<script>
  import Vue from 'vue'
  export default Vue.extend({  
        components: {
        
      },
      data () {
        return {
          valid: true,
          validation: [
              v => !!v || 'This field is required',
            ],
        }
      },
  }) 
</script>

<style>

</style>