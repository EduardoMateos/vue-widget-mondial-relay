<script>
import Vue from "vue";
import WidgetMondialRelay from "@/WidgetMondialRelay.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    WidgetMondialRelay,
  },
  methods: {
    setParcelSelected(event) {
      console.log(event)
    },
  },
});
</script>

<template>
  <div style="text-align: center">
    <div id="app" style="margin: auto;max-width: 585px;">
      <WidgetMondialRelay
        brand="BDTEST  "
        defaultPostCode="28037"
        defaultCountry="ES"
        maxResults="7"
        deliveryMode="24R"
        allowedCountries="['ES']"
        :translations="{
          headerTitle: 'Select your mondial relay ®',
          findCpText: 'CP',
          findCityText: 'City',
          cityNoResults: 'no results',
          btnListMobile: 'List',
          btnMapMobile: 'Map'
        }"
        @select="setParcelSelected($event)"
      />
    </div>
  </div>
</template>
