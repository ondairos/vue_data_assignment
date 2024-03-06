<template>
  <main class="flex flex-col lg:flex-row lg:h-screen">
    <NavBar
      :startDate="startDate"
      :endDate="endDate"
      :germanFlag="germanFlag"
      :frenchFlag="frenchFlag"
      :greekFlag="greekFlag"
      @handleStartDate="handleStartDate"
      @handleEndDate="handleEndDate"
      @clearRanges="clearRanges"
      @handleCountryToggle="handleCountryToggle"
    />

    <section v-if="loading" class="w-full h-full">
      <Spinner />
    </section>
    <section v-else class="flex flex-col w-full h-full bg-slate-200 lg:pt-0">
      <section class="flex flex-col w-full py-4 px-2 lg:h-1/2 items-center">
        <h1 class="text-center mb-1 lg:text-2xl">Energy Price to Date</h1>
        <Line
          :data="chartData"
          :options="chartOptions"
          class="shadow-lg border border-slate-200 bg-white rounded-lg"
        />
      </section>
      <section class="overflow-y-auto lg:mt-10 h-custom">
        <h1 class="text-center lg:text-2xl mb-1">Energy Prices</h1>
        <DataTable
          :filteredData="sliced_data"
          :germanFlag="germanFlag"
          :greekFlag="greekFlag"
          :frenchFlag="frenchFlag"
        />
      </section>
    </section>
  </main>
</template>

<script>
import "./App.css";
import DataTable from "./components/DataTable.vue";
import Spinner from "./components/Spinner.vue";
import NavBar from "./components/NavBar.vue";
import timeseries from "../data/timeseries.json";
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
  name: "MainPage",
  components: {
    DataTable,
    Line,
    Spinner,
    NavBar,
  },
  data() {
    return {
      loading: false,
      sliced_data: [],
      greekPrices: [],
      frenchPrices: [],
      germanPrices: [],
      allDates: [],
      startDate: null,
      endDate: null,
      greekFlag: true,
      frenchFlag: true,
      germanFlag: true,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: true,
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
  created() {
    this.sliced_data = timeseries;
    this.sliced_data.forEach((record) => {
      this.greekPrices.push(record.ENTSOE_GR_DAM_Price);
      this.frenchPrices.push(record.ENTSOE_FR_DAM_Price);
      this.germanPrices.push(record.ENTSOE_DE_DAM_Price);
      this.allDates.push(record.DateTime.split("T")[0]);
    });
  },
  methods: {
    clearRanges() {
      this.sliced_data = timeseries;
      this.allDates = [];
      this.sliced_data.forEach((record) => {
        this.allDates.push(record.DateTime.split("T")[0]);
      });
      this.startDate = null;
      this.endDate = null;
    },
    handleStartDate(startDate) {
      this.startDate = startDate;
      if (this.startDate && this.allDates.includes(this.startDate)) {
        this.sliced_data = this.sliced_data.filter((record) => {
          return record.DateTime.split("T")[0] >= this.startDate;
        });
        this.allDates = this.allDates.filter((record) => {
          return record >= this.startDate;
        });
      } else {
        alert("No record found at this start date!");
        this.startDate = null;
      }
    },
    handleEndDate(endDate) {
      this.endDate = endDate;
      if (this.endDate && this.allDates.includes(this.endDate)) {
        this.sliced_data = this.sliced_data.filter((record) => {
          return record.DateTime.split("T")[0] <= this.endDate;
        });
        this.allDates = this.allDates.filter((record) => {
          return record <= this.endDate;
        });
      } else {
        alert("No record found at this end date!");
        this.endDate = null;
      }
    },
    handleCountryToggle(country) {
      if (country === "Germany") {
        this.germanFlag = !this.germanFlag;
      } else if (country === "Greece") {
        this.greekFlag = !this.greekFlag;
      } else if (country === "France") {
        this.frenchFlag = !this.frenchFlag;
      }
    },
  },
};
</script>
