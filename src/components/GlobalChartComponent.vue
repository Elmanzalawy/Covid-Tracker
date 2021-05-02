<template>
  <div>
    <h3>Global Cases</h3>
    <div class="row mb-4">
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
    </div>
    <LineChart v-model="datacollection" v-bind:chartData="datacollection" :options="options" :key="componentKey" />
  </div>
</template>

<script>
  import LineChart from "./charts/LineChart.vue";
  import axios from 'axios'
  import moment from 'moment'

  export default {
    name: 'LineChartContainer',
    components: {
      LineChart
    },

    data() {
      return {
        componentKey: 0,
        period: {
          from: null,
          to: null
        },
        dateFrom: null,
        dateTo: null,
        datacollection: {
          labels: [],
          datasets: [{
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
            yAxes: [{
              ticks: {
                // beginAtZero: true,
                padding: 25
              }
            }]
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
      this.period.from = this.startDate;
      this.period.to = this.endDate;
      this.dateFrom = moment(new Date(this.startDate)).format("YYYY-MM-DD");
      this.dateTo = moment(new Date(this.endDate)).format("YYYY-MM-DD");
      this.getGlobalData();
    },

    mounted() {

    },
    methods: {
      getGlobalData() {
        var self = this; //assign component reference to a temporary variable (we use this for the ForEach loop) 

        //before retrieving data, we need to clear previous data if it exists:
        self.datacollection.labels = [];
        self.datacollection.datasets.forEach(dataset => {
          dataset.data = [];
        });
        
        axios
          .get(`https://api.covid19api.com/world?from=${this.period.from}&to=${this.period.to}`)
          .then(function (response) {
            var data = response.data;
            //first we need to sort records by date (asc)
            data = data.sort((a, b) => new Date(a.Date) - new Date(b.Date))
            data.forEach(record => {
              self.datacollection.labels.push(moment(new Date(record.Date)).format("MM-DD"));
              self.datacollection.datasets[0].data.push(record.NewConfirmed);
              self.datacollection.datasets[1].data.push(record.NewRecovered);
              self.datacollection.datasets[2].data.push(record.NewDeaths);
            })
            self.componentKey++;
          });
      },

      updateDateFrom(event) {
        var date = new Date(event.target.value);
        this.dateFrom = moment(date).format("YYYY-MM-DD");
        this.period.from = moment(date).format("Y-MM-DD\\Thh:mm:ss");
        this.getGlobalData();
      },
      updateDateTo(event) {
        var date = new Date(event.target.value);
        this.dateTo = moment(date).format("YYYY-MM-DD");
        this.period.to = moment(date).format("Y-MM-DD\\Thh:mm:ss");
        this.getGlobalData();
      },

      formatDate(date, format) {
        return moment(new Date(date)).format(format)
      }

    }
  }
</script>