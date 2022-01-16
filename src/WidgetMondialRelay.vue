<template>
  <div class="mondial-relay-widget">
    <MondialRelayHeader :headerTitle="t.headerTitle">
      <template v-slot:formSearchCp>
        <MondialRelayFormSearchCP
          @search="search"
          :defaultPostCode="defaultPostCode"
          :defaultCountry="defaultCountry"
          :allowedCountries="allowedCountries"
          :t="t"
        />
      </template>
    </MondialRelayHeader>

    <MondialRelayErrorMessage :message="messageError" v-if="hasError" />
    <div v-if="!hasError">
      <div class="mondial-relay-tab mondial-relay-widget-hide-desktop">
        <button
          type="button"
          :class="mobileShowMap == true ? 'active' : ''"
          @click="mobileShowMap = true"
        >
          {{ t.btnListMobile }}
        </button>
        <button
          type="button"
          :class="mobileShowMap == false ? 'active' : ''"
          @click="mobileShowMap = false"
        >
          {{ t.btnMapMobile }}
        </button>
      </div>
      <div class="mondial-relay-row">
        <div
          class="mondial-relay-left-column"
          :class="!mobileShowMap ? 'mondial-relay-widget-hide-mobile' : ''"
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
          :class="mobileShowMap ? 'mondial-relay-widget-hide-mobile' : ''"
        >
          <MondialRelayMap
            :parcelShopList="parcelShopList"
            :parcelSelected="parcelSelected"
            @selectParcel="selectParcel"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { jsonp } from "vue-jsonp";
import MondialRelayMap from "./components/MondialRelayMap";
import MondialRelayFormSearchCP from "./components/form/MondialRelayFormSearchCP";
import MondialRelayErrorMessage from "./components/MondialRelayErrorMessage";
import MondialRelayHeader from "./components/MondialRelayHeader";
import locales from "./assets/locales";

export default {
  props: {
    brand: {
      default: function() {
        return "BDTEST  ";
      },
    },
    defaultPostCode: {
      default: function() {
        return 59000;
      },
    },
    defaultCountry: {
      default: function() {
        return "FR";
      },
    },
    maxResults: {
      default: function() {
        return "7";
      },
    },
    deliveryMode: {
      default: function() {
        return "24R";
      },
    },
    allowedCountries: {
      default: function() {
        return ["FR", "ES", "BE", "NL", "LU", "DE", "AT"];
      },
    },
    translations: { type: Object, default: null },
  },
  components: {
    MondialRelayMap,
    MondialRelayFormSearchCP,
    MondialRelayErrorMessage,
    MondialRelayHeader,
  },
  name: "WidgetMondialRelay",
  computed: {
    t() {
      return {
        ...locales,
        ...this.translations,
      };
    },
  },
  data() {
    return {
      messageError: null,
      hasError: false,
      mobileShowMap: false,
      searchParcelShop: {
        Brand: this.brand,
        ClientContainerId: "Zone_Widget",
        ColLivMod: this.deliveryMode,
        Country: this.defaultCountry,
        Latitude: "",
        Longitude: "",
        NbResults: this.maxResults,
        PostCode: this.defaultPostCode,
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
      this.$emit("select", parcel);
      this.parcelSelected = parcel;
    },
    search(data) {
      if (data.cp != this.searchParcelShop.PostCode) {
        this.searchParcelShop.PostCode = data.cp;
        this.searchParcelShop.Country = data.country;
        this.getParcelShopList();
      }
    },
    getParcelShopList() {
      jsonp(
        "https://widget.mondialrelay.com/parcelshop-picker/v4_0/services/parcelshop-picker.svc/SearchPR",
        {
          callbackQuery: "method",
          callbackName: "receive",
          Brand: this.brand,
          ClientContainerId: "Zone_Widget",
          ColLivMod: this.searchParcelShop.ColLivMod,
          Country: this.searchParcelShop.Country,
          Latitude: "",
          Longitude: "",
          NbResults: this.searchParcelShop.NbResults,
          PostCode: this.searchParcelShop.PostCode,
          SearchDelay: "",
          SearchFar: "75",
          Service: "",
          VacationAfter: "",
          VacationBefore: "",
          Weight: "",
        }
      )
        .then((data) => {
          if (data.Error) {
            this.parcelShopList = [];
            this.messageError = data.Error;
            this.hasError = true;
          } else {
            this.parcelShopList = data.PRList;
            this.messageError = null;
            this.hasError = false;
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style lang="scss">
.mondial-relay-widget {
  background-color: white;
  border: solid 1px #ddd;
  max-width: 980px;
  margin: auto;
  font-family: Roboto, -apple-system, BlinkMacSystemFont, Segoe UI, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}
.mondial-relay-row {
  display: flex;
  max-height: 566px;
}

@media (max-width: 576px) {
  .mondial-relay-widget-hide-mobile {
    display: none;
  }
}

@media (min-width: 576px) {
  .mondial-relay-widget-hide-desktop {
    display: none;
  }
}

.mondial-relay-left-column {
  padding: 10px;
  @media (max-width: 576px) {
    flex: 100%;
  }
  @media (min-width: 576px) {
    flex: 35%;
  }
}
.mondial-relay-right-column {
  padding: 10px;
  @media (max-width: 576px) {
    flex: 100%;
  }
  @media (min-width: 576px) {
    flex: 65%;
  }
}
.mondial-relay-parcel-list {
  overflow-y: scroll;
  height: 450px;
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
