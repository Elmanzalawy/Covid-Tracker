<script>
import { Doughnut } from 'vue-chartjs'

export default {
    name:"DoughnutChart",
    extends: Doughnut,
    props: {
        chartData: {
            type: Object,
            default: null
        },
        options: {
            type: Object,
            default: null
        }
    },
    mounted () {
        this.renderChart(this.chartData, this.options)
    }
}
</script>
