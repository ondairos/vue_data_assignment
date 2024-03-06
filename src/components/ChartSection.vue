<template>
  <section class="flex flex-col w-full py-4 px-2 lg:h-1/2 items-center">
    <h1 class="text-center mb-1 lg:text-2xl">Energy Price to Date</h1>
    <Line
      :data="chartData"
      :options="chartOptions"
      class="shadow-lg border border-slate-200 bg-white rounded-lg"
    />
  </section>
</template>

<script>
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from "chart.js";
import { Line } from "vue-chartjs";

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

export default {
  name: "ChartSection",
  components: {
    Line,
  },
  props: {
    germanFlag: {
      type: Boolean,
    },
    greekFlag: {
      type: Boolean,
    },
    frenchFlag: {
      type: Boolean,
    },
    germanPrices: {
      type: Array,
    },
    greekPrices: {
      type: Array,
    },
    frenchPrices: {
      type: Array,
    },
    allDates: {
      type: Array,
    },
  },
  data() {
    return {
      chartOptions: {
        responsive: true,
        maintainAspectRatio: true,
        scales: {
          y: {
            ticks: {
              callback: function (value, index, ticks) {
                return new Intl.NumberFormat("en-US", {
                  style: "currency",
                  currency: "EUR",
                }).format(value);
              },
            },
          },
        },
        plugins: {
          tooltip: {
            callbacks: {
              label: function (context) {
                let label = context.dataset.label || "";
                if (label) {
                  label += ": ";
                }
                if (context.parsed.y !== null) {
                  label += new Intl.NumberFormat("en-US", {
                    style: "currency",
                    currency: "EUR",
                  }).format(context.parsed.y);
                }
                return label;
              },
            },
          },
        },
      },
    };
  },
  computed: {
    chartData() {
      const datasets = [];

      if (this.germanFlag) {
        datasets.push({
          label: "Germany",
          backgroundColor: "orange",
          borderColor: "rgba(255, 165, 0, .7)",
          data: this.germanPrices,
        });
      }
      if (this.greekFlag) {
        datasets.push({
          label: "Greece",
          backgroundColor: "blue",
          borderColor: "rgba(0, 0, 255, .7)",
          data: this.greekPrices,
        });
      }
      if (this.frenchFlag) {
        datasets.push({
          label: "France",
          backgroundColor: "red",
          borderColor: "rgba(255, 0, 0, .7)",
          data: this.frenchPrices,
        });
      }
      return {
        labels: this.allDates,
        datasets: datasets.filter((dataset) => dataset.data.length > 0),
      };
    },
  },
};
</script>
