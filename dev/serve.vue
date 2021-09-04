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
  <div id="app">
    <widget-mondial-relay brand="xxx" />
  </div>
</template>
