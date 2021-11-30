<template>
  <div>
    <!---calculator--->
    <v-card class="pa-4" outlined tile >
        <v-card-title align="center" justify="center">TNBC to USD</v-card-title>
        <v-card-text>
           <v-form v-model="valid" lazy-validation ref="form" @submit.prevent="calculate()" enctype="multipart/form-data">
                <v-text-field v-model="amountOfTNBC" label="Amount of TNBC" placeholder="10000" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-text-field v-model="rate" label="Rate" placeholder="0.02" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-card-actions>
                    <v-list-item class="grow">
                      <v-row justify="space-between">     
                        <v-btn type="submit" :disabled="!valid" @click="validate" tile>Calculate</v-btn>
                        <v-btn v-if="result != null">Total : {{ result }} USD</v-btn>
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
  export default{  
      data () {
        return {
          amountOfTNBC:'',
          rate:'',
          result: null,
          valid: true,
          validation: [
              v => !!v || 'This field is required',
            ],
        }
      },
      methods:{
        validate () {
          this.$refs.form.validate()
        },
        calculate(){
          console.log("ok");
          const tnbc = Number(this.amountOfTNBC);
          const rate = Number(this.rate);
          let total = tnbc * rate;
          this.result = total.toFixed(4);
        }
      }
  } 
</script>

<style>

</style>