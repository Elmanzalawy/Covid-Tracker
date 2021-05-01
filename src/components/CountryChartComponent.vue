<template>
  <div>
    <div class="form-group row">
    <div class="col-8"><h3>Cases in {{country}}</h3></div>
      <div class="col-4">
       <input class="form-control" list='countries' name="country" id="country" v-on:change="selectCountry($event)" placeholder="Select Country">
       <datalist id="countries">
         <!-- countries are dynamically inserted here as <option> tags -->
       </datalist>
      </div>
    </div>
    <LineChart v-model="datacollection" v-bind:chartData="datacollection" :options="options" :key="componentKey"/>
  </div>
</template>

<script>
import LineChart from "./charts/LineChart.vue";
import axios from 'axios'
import moment from 'moment'

export default {
  name: 'LineChartContainer',
    components: { LineChart },

  data(){
    return {
      componentKey: 0,
      country:"Egypt",
      countrySlug:"egypt",
      countries: [],
      datacollection: {
          labels: [],
          datasets: [
            {
              label: 'Total Cases',
              data: [],
              backgroundColor: "rgba(130, 130,0,.15)",
              borderColor: "orange",
            },
            {
              label: 'Total Recovered',
              data: [],
              backgroundColor: "rgba(71, 183,132,.15)",
              borderColor: "#47b784",
            },
            {
              label: 'Total Deaths',
              data: [],
              backgroundColor: "rgba(255, 0,0,.15)",
              borderColor: "rgb(255,0,0)",
            }
          ]
      },
      options: {
        responsive: true,
        maintainAspectRatio: false,
        // animation: false,
        scales: {
          yAxes: [
            {
              ticks: {
                // beginAtZero: true,
                padding: 25
              }
            }
          ]
        }
      }
    }
  },

created(){
  var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 
  
  //get available countries
  axios
  .get('https://api.covid19api.com/countries')
  .then(function(response){
      response.data.forEach(record => {
          self.countries.push(record);
          document.getElementById("countries").innerHTML +=`<option value="${record.Slug}">${record.Country}</option>`
      })
  });
    
  this.getCountryData();
},

  mounted() {

  },
    methods: {
      selectCountry(event){
        console.log(event.target.value)
        this.countrySlug = event.target.value;
        this.country = event.target.value;
        this.getCountryData()
      },
      
      async getCountryData(){
        var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 

        //before retrieving data, we need to clear previous data if it exists:
        self.datacollection.labels = [];
        self.datacollection.datasets.forEach(dataset => {
          dataset.data = [];
        });
        axios
        .get(`https://api.covid19api.com/total/country/${this.countrySlug}?from=2021-04-01T00:00:00Z&to=2021-04-24T00:00:00Z`)
        .then(function(response){
            response.data.forEach(record => {
                self.datacollection.labels.push(moment(new Date(record.Date)).format("M-D"));
                self.datacollection.datasets[0].data.push(record.Confirmed);
                self.datacollection.datasets[1].data.push(record.Recovered);
                self.datacollection.datasets[2].data.push(record.Deaths);
            })
            self.componentKey++;
        });
      }
    }
}
</script>
