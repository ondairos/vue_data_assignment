<template>
  <table
    class="w-full lg:w-5/6 h-full text-center mx-auto text-xs lg:text-base border border-slate-200 bg-white"
  >
    <thead class="text-xs h-8 text-white uppercase bg-gray-700">
      <th class="">Date</th>
      <th class="lg:px-10">Time</th>
      <th v-if="germanFlag">Price (Germany)</th>
      <th v-if="greekFlag">Price (Greece)</th>
      <th v-if="frenchFlag">Price (France)</th>
    </thead>
    <tr
      v-for="(record, index) in filteredData"
      :key="index"
      class="bg-white border-b"
    >
      <td class="px-2 py-2">
        {{ record.DateTime.split("T")[0].split("-").reverse().join("-") }}
      </td>
      <td class="py-2">{{ record.DateTime.split("T")[1] }}</td>
      <td v-if="germanFlag" class="py-2">{{ record.ENTSOE_DE_DAM_Price }}€</td>
      <td v-if="greekFlag" class="py-2">{{ record.ENTSOE_GR_DAM_Price }}€</td>
      <td v-if="frenchFlag" class="py-2">{{ record.ENTSOE_FR_DAM_Price }}€</td>
    </tr>
  </table>
</template>

<script>
// add loading
export default {
  name: "DataTable",
  components: {},
  props: {
    filteredData: {
      type: Array,
      required: true,
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
    return {};
  },
  mounted() {},
  computed: {
    formattedData() {
      return this.filteredData.map((item) => {
        const splitValue = item.DateTime.split("T");
        return `Date: ${splitValue[0]} Time: ${splitValue[1]}`;
      });
    },
  },
  methods: {},
};
</script>
