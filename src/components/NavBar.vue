<template>
  <nav
    class="lg:flex lg:flex-col lg:w-1/6 w-full p-4 items-center border border-r-1 border-slate-300"
  >
    <h1
      class="w-full mb-2 lg:mb-8 text-center font-bold text-3xl border-b-2 pb-2 border-black"
    >
      Energy
    </h1>
    <section class="flex flex-col h-fit w-full">
      <div
        class="flex flex-col w-full justify-between items-center lg:flex-col border-b-2 border-gray"
      >
        <div class="text-center">
          <h1 class="lg:mb-2 text-lg underline">Select a date range:</h1>
          <label for="startdate" class="hidden lg:block text-sm"
            >start date:</label
          >
          <input
            v-model="localStartDate"
            type="date"
            id="startdate"
            name="startdate"
            class="cursor-pointer"
            @change="emitStartDate"
          />
          <span class="lg:hidden mx-2"> - </span>
          <label for="enddate" class="hidden lg:block text-sm lg:mt-4"
            >end date:</label
          >
          <input
            v-model="localEndDate"
            type="date"
            id="enddate"
            name="enddate"
            class="cursor-pointer"
            @change="emitEndDate"
          />
        </div>

        <button
          type="button"
          class="text-white bg-[#24292F] mt-4 hover:bg-[#24292F]/90 font-medium rounded-lg text-sm w-32 py-2.5 text-center items-center me-2 mb-4"
          @click="emitClearDates"
        >
          Clear Dates
        </button>
      </div>
      <div
        class="flex flex-row w-full border-b-2 border-gray pb-4 items-center justify-center mt-4 lg:flex-col lg:mt-8"
      >
        <h1 class="text-center lg:mb-2 text-lg hidden underline lg:block">
          Displayed Countries
        </h1>

        <div
          v-for="(country, index) in all_countries"
          :key="index"
          class="text-center"
        >
          <CountryCheckbox
            :flag="country.flag"
            :country="country.country"
            @country-toggle="emitCountryToggle"
          />
          <label class="mr-4">{{ country.country }}</label>
        </div>
      </div>
    </section>
  </nav>
</template>

<script>
import CountryCheckbox from "./shared/CountryCheckbox.vue";

export default {
  name: "NavBar",
  components: {
    CountryCheckbox,
  },
  props: {
    startDate: {
      type: String,
    },
    endDate: {
      type: String,
    },
    germanFlag: {
      type: Boolean,
    },
    greekFlag: {
      type: Boolean,
    },
    frenchFlag: {
      type: Boolean,
    },
  },
  data() {
    return {
      localStartDate: this.startDate,
      localEndDate: this.endDate,
      all_countries: [
        {
          country: "Germany",
          flag: this.germanFlag,
        },
        {
          country: "Greece",
          flag: this.greekFlag,
        },
        {
          country: "France",
          flag: this.frenchFlag,
        },
      ],
    };
  },
  emits: [
    "handleStartDate",
    "handleEndDate",
    "clearRanges",
    "handleCountryToggle",
  ],
  methods: {
    emitStartDate() {
      this.$emit("handleStartDate", this.localStartDate);
    },
    emitEndDate() {
      this.$emit("handleEndDate", this.localEndDate);
    },
    emitClearDates() {
      this.localStartDate = null;
      this.localEndDate = null;
      this.$emit("clearRanges");
    },
    emitCountryToggle(country) {
      console.log("countr here", country);
      this.$emit("handleCountryToggle", country);
    },
  },
};
</script>
