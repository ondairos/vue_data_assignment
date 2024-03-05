<template>
  <main class="flex flex-col lg:flex-row lg:h-screen">
    <nav
      class="lg:flex lg:flex-col lg:max-w-fit w-full lg:w-40 p-4 items-center"
    >
      <h1 class="mb-2 lg:mb-8 text-center font-bold text-3xl">Energy</h1>
      <section class="flex flex-col h-fit justify-between items-center">
        <div class="flex flex-row justify-between lg:flex-col">
          <label for="startdate" class="hidden lg:block text-sm underline"
            >Pick a start date:</label
          >
          <input
            v-model="startDate"
            type="date"
            id="startdate"
            name="startdate"
            class="cursor-pointer"
            @change="handleStartDate"
          />
          <label for="enddate" class="hidden lg:block text-sm underline lg:mt-4"
            >Pick an end date:</label
          >
          <input
            v-model="endDate"
            type="date"
            id="enddate"
            name="enddate"
            class="cursor-pointer"
            @change="handleEndDate"
          />
        </div>
        <button
          type="button"
          class="text-white bg-[#24292F] mt-4 hover:bg-[#24292F]/90 font-medium rounded-lg text-sm w-32 py-2.5 text-center items-center me-2 mb-2"
          @click="changeBackground"
        >
          Search
        </button>
        <div
          class="flex flex-row justify-between items-center lg:flex-col lg:mt-10"
        >
          <p class="text-center lg:mb-2 text-sm hidden underline lg:block">
            Displayed Countries
          </p>
          <div class="text-center">
            <label for="greeceCheckbox" class="mr-1">Germany</label>
            <input type="checkbox" name="greeceCheckbox" class="mr-3" />
          </div>
          <div class="text-center">
            <label for="greeceCheckbox" class="mr-1">Greece</label>
            <input type="checkbox" name="greeceCheckbox" class="mr-3" />
          </div>
          <div class="text-center">
            <label for="greeceCheckbox" class="mr-1">France</label>
            <input type="checkbox" name="greeceCheckbox" class="mr-3" />
          </div>
        </div>
      </section>
    </nav>

    <section v-if="loading" class="w-full h-full">
      <Spinner />
    </section>
    <section v-else class="flex flex-col w-full h-full bg-slate-200 lg:pt-0">
      <section class="flex flex-col w-full py-4 lg:h-1/2 items-center">
        <!-- send array for line chart -->
        <h1 class="text-center mb-1 lg:text-2xl">Energy Price to Date</h1>
        <Line
          :data="chartData"
          :options="chartOptions"
          class="shadow-lg border border-slate-200 bg-white rounded-xl"
        />
      </section>
      <section class="overflow-y-auto lg:mt-10 h-custom">
        <h1 class="text-center lg:text-2xl mb-1">Energy Prices</h1>
        <DataTable :filteredData="sliced_data" />
      </section>
    </section>
  </main>
</template>

<script>
import "./App.css";
import DataTable from "./components/DataTable.vue";
import Spinner from "./components/Spinner.vue";
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
      chartOptions: {
        responsive: true,
        maintainAspectRatio: true,
      },
    };
  },
  computed: {
    chartData() {
      return {
        labels: this.allDates,
        datasets: [
          {
            label: "Greece",
            backgroundColor: "#0000FF",
            data: this.greekPrices,
          },
          {
            label: "France",
            backgroundColor: "red",
            data: this.frenchPrices,
          },
          {
            label: "Germany",
            backgroundColor: "yellow",
            data: this.germanPrices,
          },
        ],
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
    // console.log(this.greekPrices);
    // console.log("dates: ", this.allDates);
  },
  watch: {
    startDate(newValue, oldValue) {
      console.log("Start date changed to ", newValue);
    },
    endDate(newValue, oldValue) {},
  },
  methods: {
    changeBackground() {
      // console.log("clicked");
      this.sliced_data = this.all_data.slice(0, 10);
    },
    handleStartDate(event) {
      console.log("Selected start date ", this.startDate);
    },
    handleEndDate(event) {},
  },
};
</script>
