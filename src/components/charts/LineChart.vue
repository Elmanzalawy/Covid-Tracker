<script>
import { Line } from 'vue-chartjs'

export default {
    name:"LineChart",
    extends: Line,
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
        console.log(this.chartData)
        this.renderChart(this.chartData, this.options)
    }
}
</script>
