<script>
import Vue from "vue";
import WidgetMondialRelay from "@/widget-mondial-relay.vue";

export default Vue.extend({
  name: "ServeDev",
  components: {
    WidgetMondialRelay,
  },
});
</script>

<template>
  <div style="text-align:center;">
    <div id="app" style="max-width:980px;margin:auto">
      <widget-mondial-relay 
        brand="E1FUTBOL" 
        defaultPostCode="28032"
        defaultCountry="ES"
       />
    </div>
  </div>
</template>
