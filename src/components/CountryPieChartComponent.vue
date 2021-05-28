<template>
  <div>
    <h3>Today in {{country}}</h3>
    <!-- <small class="mb-2">Last Updated: {{lastUpdated}}</small> -->
    <PieChart v-model="datacollection" v-bind:chartData="datacollection" :options="options" :key="componentKey" />
  </div>
</template>

<script>
// https://documenter.getpostman.com/view/11144369/Szf6Z9B3?version=latest#f3782d0a-53db-4024-ab1c-95e3bd8c33ec
  import PieChart from "./charts/PieChart.vue";
  import axios from 'axios'
  import moment from 'moment'
  import {bus} from "../main";

  export default {
    name: 'PieChartContainer',
    components: {
      PieChart
    },

    props: {
      country:{
        type:String
      },
      countrySlug:{
        type:String
      },
      defaultGraphColor: {
        type:String
      }
    },

    data() {
      return {
        componentKey: 0,
        // country: "egypt",
        // countrySlug: "egypt",
        lastUpdated:null,
        period: {
          from: null,
          to: null
        },
        dateFrom: null,
        dateTo: null,
        countries: [],
        datacollection: {
          labels: ["Deaths","Recovered", "New Cases"],
          datasets: [{
            label: 'My First Dataset',
            data: [],
            backgroundColor: [
              'rgb(255, 99, 132)',
              'rgb(71, 183,132)',
              'rgb(255, 205, 86)'
            ],
            hoverOffset: 4
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          // animation: false,
          scales: {
            yAxes: [{
              ticks: {
                // beginAtZero: true,
                padding: 5,
                fontColor: this.defaultGraphColor
              }
            }]
          },
          legend: {
            labels: {
                // This more specific font property overrides the global property
                fontColor:  this.defaultGraphColor
            }
        }
        }
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

    created() {
      // var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 
      //get available countries
      bus.$on("countryChanged", (country) => {
        this.country = country;
        this.getCountryData();
      })

      this.period.from = this.startDate;
      this.period.to = this.endDate;
      this.dateFrom = moment(new Date(this.startDate)).format("YYYY-MM-DD");
      this.dateTo = moment(new Date(this.endDate)).format("YYYY-MM-DD");
      this.getCountryData();


    },

    mounted() {},
    methods: {
      async getAvailableCountries() {
        var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 
        axios
          .get('https://api.covid19api.com/countries')
          .then(function (response) {
            response.data.forEach(record => {
              self.countries.push(record);
              document.getElementById("countries").innerHTML +=
                `<option value="${record.Slug}">${record.Country}</option>`
            })
          });
      },

      selectCountry(event) {
        this.countrySlug = event.target.value;
        this.country = event.target.value;
        this.getCountryData()
      },

      async getCountryData() {
        var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 

        //before retrieving data, we need to clear previous data if it exists:
        self.datacollection.datasets.forEach(dataset => {
          dataset.data = [];
        });

        axios
          .get(
            `https://corona.lmao.ninja/v2/countries/${this.country}?yesterday=true`
          )
          .then(function (response) {
            var data = response.data;

            self.lastUpdated = moment(new Date(data.updated)).format("YYYY-MM-DD H:mm:ss");
            self.datacollection.datasets[0].data.push(data.todayDeaths);
            self.datacollection.datasets[0].data.push(data.todayRecovered);
            self.datacollection.datasets[0].data.push(data.todayCases);

            self.componentKey++;
          });
      },

      updateDateFrom(date) {
        this.dateFrom = moment(date).format("YYYY-MM-DD");
        this.period.from = moment(date).format("Y-MM-DD\\Thh:mm:ss");
      },
      updateDateTo(date) {
        this.dateTo = moment(date).format("YYYY-MM-DD");
        this.period.to = moment(date).format("Y-MM-DD\\Thh:mm:ss");
      },

    }
  }
</script>

<style scoped>

</style>