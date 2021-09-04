
<template>
  <div class="widget-mondial-relay">
    <div v-for="(item, key) in parcelShopList" :key="key">
      {{item.ID}}
    </div>
  </div>
</template>


<script>
import { jsonp } from "vue-jsonp";

export default {
  props: ['brand'],
  name: "WidgetMondialRelay",
  data() {
    return {
      searchParcelShop: {
        Brand: this.brand,
        ClientContainerId: "Zone_Widget",
        ColLivMod: "24R",
        Country: "ES",
        Latitude: "",
        Longitude: "",
        NbResults: "7",
        PostCode: "28037",
        SearchDelay: "",
        SearchFar: "75",
        Service: "",
        VacationAfter: "",
        VacationBefore: "",
        Weight: "",
      },
      parcelShopList: []
    };
  },
  created() {
    this.getParcelShopList();
  },
  methods: {
    getParcelShopList() {
      jsonp(
        "https://widget.mondialrelay.com/parcelshop-picker/v4_0/services/parcelshop-picker.svc/SearchPR",
        {
          callbackQuery: "method",
          callbackName: "receive",
          Brand: this.brand,
          ClientContainerId: "Zone_Widget",
          ColLivMod: "24R",
          Country: "ES",
          Latitude: "",
          Longitude: "",
          NbResults: "7",
          PostCode: "28037",
          SearchDelay: "",
          SearchFar: "75",
          Service: "",
          VacationAfter: "",
          VacationBefore: "",
          Weight: "",
        }
      ).then((data) => {
        this.parcelShopList = data.PRList;
      });
    },
  },
};
</script>

<style scoped>
.widget-mondial-relay {
  display: block;
  width: 400px;
  margin: 25px auto;
  border: 1px solid #ccc;
  background: #eaeaea;
  text-align: center;
  padding: 25px;
}
.widget-mondial-relay p {
  margin: 0 0 1em;
}
</style>
