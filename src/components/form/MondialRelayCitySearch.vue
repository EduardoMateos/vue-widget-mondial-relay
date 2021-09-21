<template>
  <div>
    <div class="mondial-relay-city">
      <input
        class="mondial-relay-city__input"
        id="city"
        name="city"
        @input="searchCity"
        :placeholder="findCityText"
        autocomplete="nope"
        v-model="city"
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
              {{cityNoResults}}
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
      city: "",
      cities: [],
      open: false,
    };
  },
  methods: {
    searchCity() {
      if (this.city.length > 3) {
        this.getCitiesByInput(this.city);
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
      ).then((data) => {
        this.cities = data.Value;
        this.open = true;
        console.log(data);
      });
    },
    setCity(city) {
      this.city = city.Name;
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
    padding: .375rem .75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    margin: 8px 0;
    display: inline-block;
    border: 1px solid #ced4da;
    border-radius: .25rem;
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
