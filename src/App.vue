<template>
  <div id="app">
    <Navbar />
    <div id="content-wrapper" class="container-fluid pt-2">
      <div class="form-row mb-4">
        <div class="col-lg-6">
          <div class="row">
            <div class="col-md-4 mt-2">
              <label for="date-from">From</label>
              <input name="date-from" id="date-from" class="form-control" type="date" v-model="dateFrom"
                v-on:change="updateDateFrom($event)">
            </div>
            <div class="col-md-4 mt-2">
              <label for="date-to">To</label>
              <input name="date-to" id="date-to" class="form-control" type="date" v-model="dateTo"
                v-on:change="updateDateTo($event)">
            </div>
            <div class="col-md-4 mt-2">
              <label for="country">Select Country</label>
              <!-- <input class="form-control" list='countries' name="country" id="country"
                v-on:change="selectCountry($event)" placeholder="Select Country"> -->
              <select class="form-control" list='countries' name="country" id="countries"
                v-on:change="selectCountry($event)">
                <!-- countries are dynamically inserted here as <option> tags -->
              </select>
            </div>
          </div>
        </div>
        <div class="col-md-6"></div>
      </div>

      <div class="row">
        <div class="col-md-6 pl-3 py-2">
          <!-- <GlobalChartComponent /> -->
          <CountryLineChartComponent v-bind:country="country" v-bind:countrySlug="countrySlug"/>

        </div>
        <div class="col-md-6 pl-3 py-2">
          <CountryPieChartComponent v-bind:country="country"/>
        </div>
      </div>

      <div class="row">

        <!-- <div class="col-md-4">
          <CountryPieChartComponent v-bind:country="country"/>
        </div>
        <div class="col-md-4">
          <CountryPieChartComponent v-bind:country="country"/>
        </div>
        <div class="col-md-4">
          <CountryPieChartComponent v-bind:country="country"/>
        </div> -->

      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
  import axios from 'axios'
  import moment from 'moment'
  import {bus} from "./main";
  import Navbar from './components/Navbar.vue'
  import Footer from './components/Footer.vue'
  // import GlobalChartComponent from './components/GlobalChartComponent.vue'
  import CountryLineChartComponent from './components/CountryLineChartComponent.vue'
  import CountryPieChartComponent from './components/CountryPieChartComponent.vue'

  export default {
    name: 'App',
    components: {
      Navbar,
      Footer,
      // GlobalChartComponent,
      CountryLineChartComponent,
      CountryPieChartComponent
    },

    data() {
      return {
        componentKey: 0,
        country: "Egypt",
        countrySlug: "egypt",
        period: {
          from: null,
          to: null
        },
        dateFrom: null,
        dateTo: null,
        // countries: [],
      }
    },

    computed: {
      startDate: function () {
        var startDate = new Date();
        startDate.setDate(startDate.getDate() - 30);
        return moment(new Date(startDate)).format("Y-MM-DD\\Thh:mm:ss");
      },
      endDate: function () {
        return moment(new Date).format("Y-MM-DD\\Thh:mm:ss");
      }
    },

    methods: {
      async getAvailableCountries() {
        var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 
        axios
          .get('https://api.covid19api.com/countries')
          .then(function (response) {
            response.data.forEach(record => {
              // self.countries.push(record);
              var countrySelect = document.getElementById("countries")
              var selected = false;

              if(record.Country === self.country){
                selected = true;
              }
              countrySelect.add(new Option(record.Country, record.Country, selected, selected));


            })
          });
      },

      selectCountry(event) {
        this.countrySlug = event.target.value;
        this.country = event.target.value;
        bus.$emit("countryChanged", this.country);
      },

      updateDateFrom(event) {
        var date = new Date(event.target.value);
        this.dateFrom = moment(date).format("YYYY-MM-DD");
        this.period.from = moment(date).format("Y-MM-DD\\Thh:mm:ss");
        bus.$emit("dateFromChanged", this.period.from);
      },

      updateDateTo(event) {
        var date = new Date(event.target.value);
        this.dateTo = moment(date).format("YYYY-MM-DD");
        this.period.to = moment(date).format("Y-MM-DD\\Thh:mm:ss");
        bus.$emit("dateToChanged", this.period.to);
      },

    },

    created() {
      this.period.from = this.startDate;
      this.period.to = this.endDate;
      this.dateFrom = moment(new Date(this.startDate)).format("YYYY-MM-DD");
      this.dateTo = moment(new Date(this.endDate)).format("YYYY-MM-DD");

      //get available countries
      this.getAvailableCountries();
    },

  }
</script>

<style>
  #app {
    min-height: 100vh;
    position: relative;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: white;
    background: #383838;
    padding-bottom: 5rem;
  }
</style>