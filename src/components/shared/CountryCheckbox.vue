<template>
  <input
    v-model="localFlag"
    type="checkbox"
    name="greeceCheckbox"
    class="mr-3 cursor-pointer"
    @change="emitCountryToggle(country)"
  />
</template>
<script>
export default {
  name: "CountryCheckbox",
  props: {
    flag: {
      type: Boolean,
    },
    country: {
      type: String,
    },
  },
  data() {
    return {
      localFlag: this.flag,
    };
  },
  methods: {
    emitCountryToggle(country) {
      this.$emit("country-toggle", country);
    },
  },
};
</script>
