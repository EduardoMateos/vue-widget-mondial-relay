
<template>
  <div class="mondial-relay-widget">
    <Header @searchByCp="searchByCp" />
    <div class="mondial-relay-row">
      <div class="mondial-relay-left-column">
        <div class="mondial-relay-parcel-list">
          <div
            v-for="(item, key) in parcelShopList"
            :key="key"
            class="mondial-relay-parcel-list__shop"
            @click="selectParcel(item)"
            :class="parcelSelected && parcelSelected.ID == item.ID ? 'mondial-relay-parcel-list__shop--selected':''"
          >
            {{ item.Nom }}<br />
            {{ item.Adresse1 }}<br />
            {{ item.CP }} {{ item.Ville }}
          </div>
        </div>
      </div>
      <div class="mondial-relay-right-column">
        <LMap :parcelShopList="parcelShopList" :parcelSelected="parcelSelected" />
      </div>
    </div>
  </div>
</template>


<script>
import { jsonp } from "vue-jsonp";
import LMap from "./components/LMap";
import Header from "./components/Header";
import "./assets/scss/global.scss";

export default {
  props: ["brand"],
  components: {
    LMap,
    Header,
  },
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
      parcelShopList: [],
      parcelSelected: null
    };
  },
  created() {
    this.getParcelShopList();
  },
  methods: {
    selectParcel(parcel){
      this.parcelSelected = parcel;
    },
    searchByCp(cp) {
      this.searchParcelShop.PostCode = cp;
      this.getParcelShopList();
    },
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
          PostCode: this.searchParcelShop.PostCode,
          SearchDelay: "",
          SearchFar: "75",
          Service: "",
          VacationAfter: "",
          VacationBefore: "",
          Weight: "",
        }
      ).then((data) => {
        if (data.error) {
          this.parcelShopList = [];
        } else {
          this.parcelShopList = data.PRList;
        }
      });
    },
  },
};
</script>


<style lang="scss">
.mondial-relay-widget{
  border: solid 1px #ddd;
  font-family: Roboto, -apple-system, BlinkMacSystemFont, Segoe UI, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}
.mondial-relay-row {
  display: flex;
}
.mondial-relay-left-column {
  flex: 30%;
  padding: 10px;
}
.mondial-relay-right-column {
  flex: 70%;
  padding: 10px;
}
.mondial-relay-parcel-list {
  overflow-y: scroll;
  max-height: 100%;
  &__shop {
    padding: 8px;
    cursor: pointer;
    border-bottom: 1px solid #c4c4c4;
    text-align: left;
    &--selected{
      background-color: #f0f0f0;
    }
  }
}
</style>
