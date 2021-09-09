
<template>
  <div class="mondial-relay-widget">
    <MondialRelayHeader>
      <template v-slot:formSearchCp>
        <MondialRelayFormSearchCP @searchByCp="searchByCp" />
      </template>
    </MondialRelayHeader>
    
    <MondialRelayErrorMessage :message="messageError" v-if="hasError" />
    <div v-show="!hasError">
      <div class="mondial-relay-tab hide-desktop">
        <button
          :class="mobileShowMap == true ? 'active' : ''"
          @click="mobileShowMap = true"
        >
          Listado
        </button>
        <button
          :class="mobileShowMap == false ? 'active' : ''"
          @click="mobileShowMap = false"
        >
          Mapa
        </button>
      </div>
      <div class="mondial-relay-row">
        <div
          class="mondial-relay-left-column"
          :class="!mobileShowMap ? 'hide-mobile' : ''"
        >
          <div class="mondial-relay-parcel-list">
            <div
              v-for="(item, key) in parcelShopList"
              :key="key"
              class="mondial-relay-parcel-list__shop"
              @click="selectParcel(item)"
              :class="
                parcelSelected && parcelSelected.ID == item.ID
                  ? 'mondial-relay-parcel-list__shop--selected'
                  : ''
              "
            >
              <div class="mondial-relay-parcel-list__shop__name">
                {{ item.Nom }}
              </div>
              {{ item.Adresse1 }}<br />
              {{ item.CP }} {{ item.Ville }}
            </div>
          </div>
        </div>
        <div
          class="mondial-relay-right-column"
          :class="mobileShowMap ? 'hide-mobile' : ''"
        >
          <LMap
            :parcelShopList="parcelShopList"
            :parcelSelected="parcelSelected"
          />
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import { jsonp } from "vue-jsonp";
import LMap from "./components/LMap";
import MondialRelayFormSearchCP from "./components/form/MondialRelayFormSearchCP";
import MondialRelayErrorMessage from "./components/MondialRelayErrorMessage";
import MondialRelayHeader from "./components/MondialRelayHeader";
import "./assets/scss/global.scss";

export default {
  props: ["brand"],
  components: {
    LMap,
    MondialRelayFormSearchCP,
    MondialRelayErrorMessage,
    MondialRelayHeader,
  },
  name: "WidgetMondialRelay",
  data() {
    return {
      messageError: null,
      hasError: false,
      mobileShowMap: true,
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
      parcelSelected: null,
    };
  },
  created() {
    this.getParcelShopList();
  },
  methods: {
    selectParcel(parcel) {
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
        if (data.Error) {
          this.parcelShopList = [];
          this.messageError = data.Error;
          this.hasError = true;
        } else {
          this.parcelShopList = data.PRList;
          this.messageError = null;
          this.hasError = false;
        }
      });
    },
  },
};
</script>


<style lang="scss">
.mondial-relay-widget {
  border: solid 1px #ddd;
  font-family: Roboto, -apple-system, BlinkMacSystemFont, Segoe UI, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}
.mondial-relay-row {
  display: flex;
}

@media (max-width: 576px) {
  .hide-mobile {
    display: none;
  }
}

@media (min-width: 576px) {
  .hide-desktop {
    display: none;
  }
}

.mondial-relay-left-column {
  padding: 10px;
  @media (max-width: 576px) {
    flex: 100%;
  }
  @media (min-width: 576px) {
    flex: 30%;
  }
}
.mondial-relay-right-column {
  padding: 10px;
  @media (max-width: 576px) {
    flex: 100%;
  }
  @media (min-width: 576px) {
    flex: 70%;
  }
}
.mondial-relay-parcel-list {
  overflow-y: scroll;
  max-height: 100%;
  &__shop {
    padding: 8px;
    cursor: pointer;
    border-bottom: 1px solid #c4c4c4;
    text-align: left;
    &__name {
      color: #ca0047;
    }
    &--selected {
      background-color: #f0f0f0;
      border-left: solid 2px #ca0047;
    }
  }
}

.mondial-relay-tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
  button {
    background-color: inherit;
    float: center;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 14px 16px;
    transition: 0.3s;
    font-size: 17px;
    &:hover {
      background-color: #ddd;
    }
    &.active {
      background-color: #ccc;
    }
  }
}
</style>
