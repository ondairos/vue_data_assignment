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
      <ChartSection
        :germanFlag="germanFlag"
        :greekFlag="greekFlag"
        :frenchFlag="frenchFlag"
        :germanPrices="germanPrices"
        :greekPrices="greekPrices"
        :frenchPrices="frenchPrices"
        :allDates="allDates"
      />
      <section class="overflow-y-auto lg:mt-10 h-custom">
        <h1 class="text-center lg:text-2xl mb-1">Energy Prices</h1>
        <DataTable
          :filteredData="all_data"
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
import Spinner from "./components/shared/Spinner.vue";
import NavBar from "./components/shared/NavBar.vue";
import ChartSection from "./components/ChartSection.vue";
import timeseries from "../data/timeseries.json";

export default {
  name: "MainPage",
  components: {
    DataTable,
    Spinner,
    NavBar,
    ChartSection,
  },
  data() {
    return {
      loading: false,
      all_data: [],
      greekPrices: [],
      frenchPrices: [],
      germanPrices: [],
      allDates: [],
      startDate: null,
      endDate: null,
      greekFlag: true,
      frenchFlag: true,
      germanFlag: true,
    };
  },
  created() {
    this.all_data = timeseries;
    this.all_data.forEach((record) => {
      this.greekPrices.push(record.ENTSOE_GR_DAM_Price);
      this.frenchPrices.push(record.ENTSOE_FR_DAM_Price);
      this.germanPrices.push(record.ENTSOE_DE_DAM_Price);
      this.allDates.push(record.DateTime.split("T")[0]);
    });
  },
  methods: {
    clearRanges() {
      this.all_data = timeseries;
      this.allDates = [];
      this.all_data.forEach((record) => {
        this.allDates.push(record.DateTime.split("T")[0]);
      });
      this.startDate = null;
      this.endDate = null;
    },
    handleStartDate(startDate) {
      this.startDate = startDate;
      if (this.startDate && this.allDates.includes(this.startDate)) {
        this.all_data = this.all_data.filter((record) => {
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
        this.all_data = this.all_data.filter((record) => {
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
