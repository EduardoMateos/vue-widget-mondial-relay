
<template>
  <div class="mondial-relay-form">
    <div class="mondial-relay-form__countries">
      <MondialRelayCountrySelector @changeCountry="changeCountry" :countrySelected="country"></MondialRelayCountrySelector>
    </div>
    <div class="mondial-relay-form__cp">
      <input
        class="mondial-relay-input"
        id="cp"
        name="cp"
        v-model="cp"
        required
        placeholder="CP"
      />
    </div>
    <div class="mondial-relay-form__search">
      <img src="./../../assets/images/search.svg" v-on:click="search()" />
    </div>
  </div>
</template>


<script>
import MondialRelayCountrySelector from "./MondialRelayCountrySelector";
export default {
  props: ['defaultPostCode', 'defaultCountry'],
  name: "Header",
  components: {
    MondialRelayCountrySelector,
  },
  data() {
    return {
      cp: this.defaultPostCode,
      country: this.defaultCountry
    };
  },
  methods: {
    search() {
      this.$emit("search", {
        cp: this.cp,
        country: this.country
      });
    },
    changeCountry(country){
      this.country = country
    }
  },
};
</script>

<style lang="scss">
.mondial-relay-form {
  margin: auto;
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
    width: 22px;
    vertical-align: middle;
    cursor: pointer;
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
