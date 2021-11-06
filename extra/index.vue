<template>
  <div>
     <!--- input box --->
    <v-form @submit.prevent="submit()" enctype="multipart/form-data">
      <v-row>
        <v-col cols="12" sm="4" md="6">
        <v-card>
          <v-card-text>
            <v-text-field v-model="name" label="name" placeholder="Sazid" type="text" class="rounded-0" outlined required></v-text-field>
            <v-text-field v-model="age" label="age" placeholder="30" type="number" class="rounded-0" outlined required></v-text-field>
            <v-select
              :items="items"
              label="Select"
              class="rounded-0"
              v-model="choice"
              outlined
            ></v-select>
          </v-card-text>
        </v-card>
        </v-col>
        <v-col cols="12" sm="4" md="6">
          <v-card class="pa-4">
            <v-radio-group v-model="radios">
              <template v-slot:label>
                <div>Marrital Status</div>
              </template>
              <v-radio value="Married">
                <template v-slot:label>
                  <div>Married</div>
                </template>
              </v-radio>
              <v-radio value="Unmarried">
                <template v-slot:label>
                  <div>Unmarried</div>
                </template>
              </v-radio>
            </v-radio-group>
            <v-file-input
              label="Upload File"
              outlined
              dense
              prepend-icon="mdi-camera"
              accept="image/*"
            ></v-file-input>
            <!-- date time -->
            <v-row>
              <!-- date -->
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  :return-value.sync="date"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="date"
                      label="Select Date"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="date"
                    no-title
                    scrollable
                  >
                    <v-spacer></v-spacer>
                    <v-btn
                      text
                      color="primary"
                      @click="menu = false"
                    >
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menu.save(date)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
              </v-col>
              <!-- time -->
              <v-col
                cols="12"
                sm="6"
                md="4"
              >
                <v-menu
                  ref="menu"
                  v-model="menu2"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  :return-value.sync="time"
                  transition="scale-transition"
                  offset-y
                  max-width="290px"
                  min-width="290px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="time"
                      label="Select Time"
                      prepend-icon="mdi-clock-time-four-outline"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-time-picker
                    v-if="menu2"
                    v-model="time"
                    full-width
                    @click:minute="$refs.menu.save(time)"
                  ></v-time-picker>
                </v-menu>
              </v-col>
            </v-row>
          </v-card>
        </v-col>
      </v-row>
      <div class="white--text">
        <v-btn type="submit"
        color="primary" block tile>Submit</v-btn>
      </div>
    </v-form>
      <!--- End input box --->
  </div>
</template>

<script>
export default {
  data: () => ({
    name: 's',
    age:'30',
    choice : '',
    items: ['React', 'Vue', 'Angular'],
    radios: 'Married',
    date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
    menu: false,
    time: null,
    menu2: false,
    image: null,
  }),

  mounted() {
    //
  },
  created() {
    //
  },
  methods:{
    createData() {
      const config = {
        headers: {
          "content-type": "multipart/form-data",
        },
      };
      let formData = new FormData();
      formData.append("name", this.name);
      formData.append("age", this.age);
      formData.append("choice", this.choice);
      formData.append("married", this.radios);
      formData.append("date", this.date);
      formData.append("time", this.time);
      formData.append("image", this.image);
      // fetch('http://localhost:3000/jobs', formData, config)
      //   .then(res => res.json())
      //   .then(data => this.jobs = data)
      //   .catch(err => console.log(err.message))

      console.log(formData);
    },

    submit(){
      console.log(
        this.name,
        this.age,
        this.choice,
        this.radios,
        this.date,
        this.time,
        this.image,
        this.image.name
      );
    }
  }
}
</script>

<style>

</style>