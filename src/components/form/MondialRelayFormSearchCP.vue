
<template>
  <form method="post" @submit="search" class="mondial-relay-form">
    <div class="mondial-relay-form__city">
      <MondialRelayCitySearch
        @setPostCode="changeCP"
        :country="country"
        :findCityText="t.findCityText"
        :cityNoResults="t.cityNoResults"
      ></MondialRelayCitySearch>
    </div>
    <div class="mondial-relay-form__countries">
      <MondialRelayCountrySelector
        @changeCountry="changeCountry"
        :countrySelected="country"
        :allowedCountries="allowedCountries"
      ></MondialRelayCountrySelector>
    </div>
    <div class="mondial-relay-form__cp">
      <input
        class="mondial-relay-input"
        id="cp"
        name="cp"
        v-model="cp"
        required
        :placeholder="t.findCpText"
      />
    </div>

    <button class="mondial-relay-form__search">
      <img src="@/assets/images/search.svg" alt="search" />
    </button>
  </form>
</template>


<script>
import MondialRelayCountrySelector from "./MondialRelayCountrySelector";
import MondialRelayCitySearch from "./MondialRelayCitySearch";

export default {
  props: ["defaultPostCode", "defaultCountry", "allowedCountries", "t"],
  name: "Header",
  components: {
    MondialRelayCountrySelector,
    MondialRelayCitySearch,
  },
  data() {
    return {
      cp: this.defaultPostCode,
      country: this.defaultCountry,
    };
  },
  methods: {
    async search(e) {
      this.$emit("search", {
        cp: this.cp,
        country: this.country,
      });
      e.preventDefault();
    },
    changeCountry(country) {
      this.country = country;
    },
    changeCP(postCode) {
      this.cp = postCode;
      this.$emit("search", {
        cp: postCode,
        country: this.country,
      });
    },
  },
};
</script>

<style lang="scss">
.mondial-relay-form {
  &__city {
    margin-right: 12px;
    @media (min-width: 576px) {
      display: inline-block;
    }
        @media (max-width: 576px) {
      margin-bottom:8px;
    }
  }
  &__countries {
    display: inline-block;
    width: 78px;

  }
  &__cp {
    display: inline-block;
    width: 52px;
    margin-right: 28px;
  }
  &__search {
    display: inline-block;
    margin-left: 12px;
    width: 38px;
    vertical-align: middle;
    cursor: pointer;
    background-color: white;
    color: #fff;
    border: 0;
  }
}

.mondial-relay-input {
  position: relative;
  display: block;
  padding: 0.375rem 0.75rem;
  font-size: 1rem;
  line-height: 1.5;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  max-width: 100%;

  &__toggle {
    position: absolute;
    right: 5px;
    top: calc(50% - 10px);
    transition: all 0.25s cubic-bezier(0.645, 0.045, 0.355, 1);
    text-align: center;
    display: inline-block;
    cursor: pointer;
    height: 24px;
  }

  &:focus {
    color: #495057;
    background-color: #fff;
    border-color: black;
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgb(0 123 255 / 25%);
  }

  &__flag {
    position: absolute;
    top: 8px;
    left: 14px;
  }
}
</style>
