<template>
    <div>
      <!--- input box --->
      <v-row>
        <v-col cols="12" sm="4" md="6">
        <v-card>
          <v-card-text>
              <v-form v-model="valid" lazy-validation ref="form" @submit.prevent="calculate()" enctype="multipart/form-data">
                <v-text-field v-model="interval" label="Intervals" placeholder="6" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-text-field v-model="invest" label="Base Investment" placeholder="100" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-text-field v-model="rate" label="Initial Rate" placeholder="0.01" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
                <v-text-field v-model="rateIncrement" label="Increment Rate" placeholder="0.001" type="number"  class="rounded-0" :rules="validation" outlined required></v-text-field>
                <div class="white--text">
                  <v-btn type="submit" :disabled="!valid"
                  @click="validate"
                  color="primary" block tile>Calculate</v-btn>
                </div>
              </v-form>
          </v-card-text>
        </v-card>
        </v-col>
        <v-col cols="12" sm="4" md="6">
          <v-card class="pa-4" v-if="show">
            <h3 class="pb-5">Results</h3>
            <p>Intervals : {{ this.data.interval }}</p>
            <p>Base Investment : {{ this.data.invest }}</p>
            <p>Initial Rate : {{ this.data.rate }}</p>
            <p>Rate On This Interval : {{ this.data.dayRate }}</p>
            <p>Total Investment : {{ this.data.total_Investment }}</p>
            <p>Total TNBC : {{ this.data.total_crypto }}</p>
            <p>Total Price : {{ this.data.sale_price }}</p>
            <p>Profit : {{ this.data.profit }}</p>
          </v-card>
          <v-card class="pa-4" v-else>
            <h3 class="pb-5">Instructions</h3>
            <h4>Intervals</h4>
            <p>How many times you want to invest.</p>
            <h4>Base Investment</h4>
            <p>A fixed amount that you want to invest each time.</p>
            <h4>Initial Rate</h4>
            <p>The rate that you want to start the investment.</p>
            <h4>Increment Rate</h4>
            <p>It determines the next interval rate. For example: If initial rate is 0.01 and you want to invest again when the price will would be 0.011, than the increament rate would be 0.001. On each interval the rate would be increamented by 0.001 and the calculation would be based on this rate.</p>
          </v-card>
        </v-col>
      </v-row>
      <!--- End input box --->
      <!-- Data Table-->
      <div v-if="show" class="pt-5">
        <v-card>
          <v-toolbar flat >
            <v-toolbar-title>Calculation Details</v-toolbar-title>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-text-field v-model="search" append-icon="mdi-magnify" label="Search" color="deep-purple" single-line hide-details ></v-text-field>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-btn color="cyan" small dark class="mb-2">Download PDF</v-btn>
            <v-divider class="mx-4" inset vertical></v-divider>
          </v-toolbar>
          <!-- datatable -->                        
          <v-data-table class="elevation-0" :headers="headers" :items="data.blocks" :search="search" :loading="loading"
              loading-text="Loading... Please wait" :footer-props="{itemsPerPageOptions: [5,10,15],itemsPerPageText: 'Data Per Page','show-current-page': true,'show-first-last-page': true}">
              <template v-slot:[`item.actions`]="{ item }" >
                  <v-icon small color="cyan" class="mr-2" @click="editBtn(item.id)"> mdi-pencil </v-icon>
                  <v-icon small color="red"  @click="deleteBtn(item.id)"> mdi-delete </v-icon>
              </template>
          </v-data-table>
          <!-- End datatable -->
        </v-card>
      </div>
    </div>
</template>

<script>
export default {
     data () {
          return {
            valid: true,
            show:false,
            search: '',
            loading: false,
            interval: '',
            invest : '',
            rate : '',
            rateIncrement : '',
            data: [],
            calculations:[],
            headers: [
              { text: 'Intervals', align: 'start', sortable: true, value: 'interval'},
              { text: 'Investment', align: 'start', sortable: false, value: 'invest'},
              { text: 'Rate', align: 'start', sortable: true, value: 'rate'},
              { text: 'Tnbc', align: 'start', sortable: false, value: 'tnbc'},
              { text: 'Total TNBC', align: 'start', value: 'totalCrypto', sortable: false },
              { text: 'Total Investment', align: 'start', value: 'totalInvest', sortable: false },
          ],
          validation: [
            v => !!v || 'This field is required',
          ],
         }
          
      },
      mounted() {
        //
      },
      created() {
        //
      },
      methods:{
        blocks(interval, invest, rate, rateIncrement){
          let blocks = [];
          let totalCrypto = 0;
          let totalInvest = 0;

          for(let i =1; i <= interval; i++){
            let tnbc = invest / rate;
            totalCrypto = totalCrypto + tnbc;
            totalInvest = totalInvest + invest

            let obj = {
                "interval" : i,
                "invest" : invest,
                "rate" : rate.toFixed(3),
                "tnbc" : tnbc.toFixed(3),
                "totalCrypto" : totalCrypto.toFixed(3),
                "totalInvest" : totalInvest
            }

            blocks.push(obj);
            rate = rate + rateIncrement;
          }
          return blocks;
        },
        validate () {
          this.$refs.form.validate()
        },
        profit(interval, invest, rate, rateIncrement){
          let n = interval - 1;
          let blocks = this.blocks(interval, invest, rate, rateIncrement);
          let block = blocks[n];
          let dayRate = block.rate;
          let totalCrypto = block.totalCrypto;
          let totalInvest = block.totalInvest;

          let salePrice = dayRate * totalCrypto;
          let profit = salePrice - totalInvest;

          let obj = {
            "blocks" : blocks,
            "interval" : interval,
            "invest" : invest,
            "rate" : rate,
            "dayRate" : dayRate,
            "total_Investment" : totalInvest,
            "total_crypto" : totalCrypto,
            "sale_price" : salePrice.toFixed(2),
            "profit" : profit.toFixed(2),
          }
          return obj;
        },
        calculate(){
          const interval = Number(this.interval);
          const invest = Number(this.invest);
          const rate = Number(this.rate);
          const rateIncrement = Number(this.rateIncrement);

          // const interval = 2;
          // const invest = Number(100);
          // const rate = Number(0.01);
          // const rateIncrement = Number(0.001);

          let profit = this.profit(interval, invest, rate, rateIncrement);
          this.data = profit;
          this.show = true;
        }
      }
}
</script>

<style>

</style>