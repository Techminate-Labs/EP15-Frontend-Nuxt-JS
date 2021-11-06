<template>
  <div>
    <!--- slider--->
      <v-carousel>
        <v-carousel-item v-for="(slide, i) in slides" :key="i">
          <v-img :src="src[i]" height="100%">
            <v-row
              class="fill-height"
              align="center"
              justify="center"
            >
              <div class="text-h2">
                {{ slide }} Slide
              </div>
            </v-row>
          </v-img>
        </v-carousel-item>
      </v-carousel>
    <!--- End slider --->
    <!---calculator 1--->
    <v-row class="ma-5">
      <v-col cols="12" sm="6" md="6">
        <Calculator1 />
      </v-col>

      <!---Calculator 2--->
      <v-col cols="12" sm="6" md="6">
        <Calculator2 />
      </v-col>
    </v-row>
    <!---end calculator--->
    <!---Investmen plan--->
      <Investmen />
    <!--End Investmen plan--->
  </div>
</template>

<script>
  import Vue from 'vue'
  import Calculator1 from '@/components/Calculator1.vue'
  import Calculator2 from '@/components/Calculator2.vue'
  import Investmen from '@/components/Investmen.vue'
  export default Vue.extend({  
        components: {
        Calculator1,
        Calculator2,
        Investmen
      },
      data () {
        return {
          valid: true,
          src: [
            'https://image.shutterstock.com/image-vector/web-development-coding-cross-platform-260nw-1128653108.jpg',
            'https://images.pexels.com/photos/69432/pexels-photo-69432.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500',
            'https://images.pexels.com/photos/1029757/pexels-photo-1029757.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500',
          ],
          slides: [
            'First',
            'Second',
            'Third',
          ],
        }
      },
  }) 
</script>

<style>

</style>





import Vue from 'vue'
import Categories from '@/components/storefront/products/categories/Categories.vue'
import Slider from '@/components/storefront/products/Slider.vue'
export default Vue.extend({
  components: {
    Categories,
    Slider
  },