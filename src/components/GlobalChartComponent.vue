<template>
  <div>
    <h3>Global Cases</h3>
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
      max_number_of_readings: 10,
      datacollection: {
          labels: [],
          datasets: [
            {
              label: 'New Cases',
              data: [],
              backgroundColor: "rgba(130, 130,0,.15)",
              borderColor: "orange",
            },
            {
              label: 'Recovered',
              data: [],
              backgroundColor: "rgba(71, 183,132,.15)",
              borderColor: "#47b784",
            },
            {
              label: 'Deaths',
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
    axios
    .get('https://api.covid19api.com/world?from=2021-03-01T00:00:00Z&to=2021-04-01T00:00:00Z')
    .then(function(response){
        response.data.forEach(record => {
            self.datacollection.labels.push(moment(new Date(record.Date)).format("M-D"));
            self.datacollection.datasets[0].data.push(record.NewConfirmed);
            self.datacollection.datasets[1].data.push(record.NewRecovered);
            self.datacollection.datasets[2].data.push(record.NewDeaths);
        })
        self.componentKey++;
    });
},

  mounted() {

  },
    methods: {

    }
}
</script>
