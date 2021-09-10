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
    <div id="app" style="max-width: 980px; margin: auto">
      <WidgetMondialRelay
        brand="BDTEST  "
        defaultPostCode="59000"
        defaultCountry="FR"
        maxResults="7"
        deliveryMode="24R"
        @select="setParcelSelected($event)"
      />
    </div>
  </div>
</template>
