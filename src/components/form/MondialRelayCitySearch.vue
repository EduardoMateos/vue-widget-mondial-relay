<template>
  <div>
    <div class="mondial-relay-city">
      <input
        class="mondial-relay-city__input"
        id="search"
        name="search"
        @input="searchCity"
        :placeholder="findCityText"
        autocomplete="nope"
        v-model="search"
        @blur="resetField"
      />
      <transition name="fade">
        <div
          class="mondial-relay-city__dropdown"
          v-if="open"
          v-click-outside="close"
        >
          <ul v-if="this.cities.length > 0">
            <li v-for="(city, key) in cities" :key="key" @click="setCity(city)">
              {{ city.Name }} ({{ city.PostCode }})
            </li>
          </ul>
          <ul v-if="this.cities.length == 0">
            <li @click="close">
              {{ cityNoResults }}
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { jsonp } from "vue-jsonp";
import ClickOutside from "vue-click-outside";

export default {
  props: ["country", "findCityText", "cityNoResults"],
  data() {
    return {
      search: "",
      citySelected: {},
      cities: [],
      open: false,
      debounce: null
    };
  },
  methods: {
    resetField() {
      if (this.citySelected.Name != this.search) {
        this.search = "";
      }
    },
    searchCity() {
      if (this.search.length > 3) {
        clearTimeout(this.debounce);
        this.debounce = setTimeout(() => {
          this.getCitiesByInput(this.search);
        }, 600);
      }
    },
    getCitiesByInput(input) {
      jsonp(
        "https://widget.mondialrelay.com/parcelshop-picker/v4_0/services/parcelshop-picker.svc/AutoCPLCity",
        {
          callbackQuery: "method",
          callbackName: "receive",
          Country: this.country,
          City: input,
        }
      )
        .then((data) => {
          this.cities = data.Value;
          this.open = true;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    setCity(city) {
      this.search = city.Name;
      this.citySelected = city;
      this.$emit("setPostCode", city.PostCode);
      this.open = false;
    },
    close() {
      if (this.open) {
        this.open = false;
      }
    },
  },
  directives: {
    ClickOutside,
  },
};
</script>


<style lang="scss">
.mondial-relay-city {
  position: relative;
  &__input {
    width: 100%;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    box-sizing: border-box;
    &:focus {
      color: #495057;
      background-color: #fff;
      border-color: black;
      outline: 0;
      box-shadow: 0 0 0 0.2rem rgb(0 123 255 / 25%);
    }
  }

  &__dropdown {
    position: absolute;
    background-color: white;
    width: 100%;
    text-align: left;
    z-index: 500;
    border-right: 1px solid #ced4da;
    border-bottom: 1px solid #ced4da;
    border-left: 1px solid #ced4da;
    ul {
      list-style-type: none;
      padding: 0px;
      margin: 0px;
      li {
        padding: 6px;
        cursor: pointer;
        &:hover {
          background-color: #f0f0f0;
          border-left: solid 2px #ca0047;
        }
      }
    }
    &--selected {
      background-color: #f0f0f0;
      border-left: solid 2px #ca0047;
    }
  }
}
</style>
