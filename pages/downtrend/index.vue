<template>
    <div class="ma-5">
      <!--- input box --->
      <v-row>
        <v-col cols="12" sm="12" md="6" lg="6" xl="6">
          <v-card class="text-center" v-if="show">
            <v-card-text>
              <v-sparkline
                :labels="labels"
                :value="value"
                :gradient="gradient"
                :smooth="radius || false"
                :padding="padding"
                :line-width="width"
                :stroke-linecap="lineCap"
                :gradient-direction="gradientDirection"
                :fill="fill"
                :type="type"
                :auto-line-width="autoLineWidth"
                auto-draw
              ></v-sparkline>
            </v-card-text>
            <v-divider></v-divider>
            <v-card-text>
              <div class="pt-5 pb-5 text-h5 font-weight-thin">
                Total TNBC On  Each Interval
              </div>
            </v-card-text>
          </v-card>
        <v-card v-else>
          <v-card-text>
            <v-form v-model="valid" lazy-validation ref="form" @submit.prevent="calculate()" enctype="multipart/form-data">
              <v-text-field v-model="interval" label="Intervals" placeholder="6" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
              <v-text-field v-model="invest" label="Base Investment" placeholder="100" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
              <v-text-field v-model="rate" label="Initial Rate" placeholder="0.01" type="number" class="rounded-0" :rules="validation" outlined required></v-text-field>
              <v-text-field v-model="rateDecrement" label="Decrement Rate" placeholder="0.001" type="number"  class="rounded-0" :rules="validation" outlined required></v-text-field>
              <div class="white--text">
                <v-btn type="submit" :disabled="!valid"
                @click="validate"
                block tile>Calculate</v-btn>
              </div>
            </v-form>
          </v-card-text>
        </v-card>
        </v-col>
        <v-col cols="12" sm="12" md="6" lg="6" xl="6">
          <v-card class="pa-4" v-if="show">
            <h3 class="pb-5">Results</h3>
            <p>Intervals : {{ this.data.interval }}</p>
            <p>Base Investment : {{ this.data.invest }}</p>
            <p>Total Investment : {{ this.data.total_Investment }}</p>
            <p>Initial Rate : {{ this.data.rate }}</p>
            <p>Rate On {{ interval }}th Interval : {{ this.data.dayRate }}</p>
            <p>Total TNBC On {{ interval }}th Interval : {{ this.data.total_crypto }}</p>
            <p>Total Price On {{ interval }}th Interval : {{ this.data.sale_price }}</p>
            <p>Total PNL : {{ this.data.profit }}</p>
          </v-card>
          <v-card class="pa-4" v-else>
            <h3 class="pb-5">Instructions</h3>
            <h4>Intervals</h4>
            <p>How many times you want to invest.</p>
            <h4>Base Investment</h4>
            <p>A fixed amount that you want to invest each time.</p>
            <h4>Initial Rate</h4>
            <p>The rate that you want to start the investment.</p>
            <h4>Decrement Rate</h4>
            <p>It determines the next interval rate. For example: If initial rate is 0.015 and you want to invest again when the price will be decreased to 0.014, than the decreament rate would be 0.001. On each interval the rate would be decreamented by 0.001 and the calculation would be done following this propotion.</p>
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
            <v-btn small class="mb-2" @click="clear()">Clear</v-btn>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-btn small class="mb-2"><v-icon>mdi-arrow-down-bold-box-outline</v-icon></v-btn>
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
const gradients = [
    ['#222'],
    ['#42b3f4'],
    ['red', 'orange', 'yellow'],
    ['purple', 'violet'],
    ['#00c6ff', '#F0F', '#FF0'],
    ['#f72047', '#ffd200', '#1feaea'],
  ]
export default {
     data () {
          return {
            width: 2,
            radius: 10,
            padding: 8,
            lineCap: 'round',
            gradient: gradients[5],
            labels: [],
            value: [],
            gradientDirection: 'top',
            gradients,
            fill: true,
            type: 'bar',
            autoLineWidth: false,

            valid: true,
            show:false,
            search: '',
            loading: false,
            interval: '',
            invest : '',
            rate : '',
            rateDecrement : '',
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
      methods:{
         clear(){
          console.log('clear');
          this.data = [];
          this.labels = [];
          this.value = [];
          this.show = false;
        },
        blocks(interval, invest, rate, rateDecrement){
          let blocks = [];
          let totalCrypto = 0;
          let totalInvest = 0;

          for(let i =1; i <= interval; i++){
            let tnbc = invest / rate;
            totalCrypto = totalCrypto + tnbc;
            totalInvest = totalInvest + invest
            let floatRate = rate.toFixed(3);

            let obj = {
              "interval" : i,
              "invest" : invest,
              "rate" : floatRate,
              "tnbc" : tnbc.toFixed(3),
              "totalCrypto" : totalCrypto.toFixed(3),
              "totalInvest" : totalInvest
            }

            blocks.push(obj);
            this.labels.push(floatRate);
            this.value.push(totalCrypto);
            rate = rate - rateDecrement;
          }
          return blocks;
        },
        validate () {
          this.$refs.form.validate()
        },
        profit(interval, invest, rate, rateDecrement){
          let n = interval - 1;
          let blocks = this.blocks(interval, invest, rate, rateDecrement);
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
          const rateDecrement = Number(this.rateDecrement);

          // const interval = 2;
          // const invest = Number(100);
          // const rate = Number(0.01);
          // const rateDecrement = Number(0.001);

          let profit = this.profit(interval, invest, rate, rateDecrement);
          this.data = profit;
          this.show = true;
        }
      }
}
</script>

<style>

</style>