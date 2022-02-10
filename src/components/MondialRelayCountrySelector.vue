<template>
  <div>
    <div class="mondial-relay-selector">
      <input
        class="mondial-relay-selector__input"
        id="cp"
        readonly="readonly"
        name="cp"
        required
        size="8"
        v-on:click="open = !open"
      />
      <div
        class="mondial-relay-selector__input__flag"
        v-on:click="open = !open"
      >
        <img v-if="countrySelected == 'ES'" src="@/assets/images/es-flag.svg" width="30px" />
        <img v-if="countrySelected == 'AT'" src="@/assets/images/at-flag.svg" width="30px" />
        <img v-if="countrySelected == 'FR'" src="@/assets/images/fr-flag.svg" width="30px" />
        <img v-if="countrySelected == 'DE'" src="@/assets/images/de-flag.svg" width="30px" />
        <img v-if="countrySelected == 'LU'" src="@/assets/images/lu-flag.svg" width="30px" />
        <img v-if="countrySelected == 'NL'" src="@/assets/images/nl-flag.svg" width="30px" />
        <img v-if="countrySelected == 'BE'" src="@/assets/images/be-flag.svg" width="30px" />
      </div>
      <div
        class="mondial-relay-selector__input__toggle"
        v-on:click="open = !open"
      >
        <svg
          data-v-46e105de=""
          mlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          class="country-selector__toggle__arrow"
        >
          <path
            data-v-46e105de=""
            d="M7.41 8.59L12 13.17l4.59-4.58L18 10l-6 6-6-6 1.41-1.41z"
            class="arrow"
          ></path>
          <path data-v-46e105de="" fill="none" d="M0 0h24v24H0V0z"></path>
        </svg>
      </div>
      <transition name="fade">
        <div class="mondial-relay-selector__dropdown" v-if="open">
          <ul>
            <li
            v-if="allowedCountries.includes('FR')"
              :class="
                countrySelected == 'FR'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('FR')"
            >
              <img src="@/assets/images/fr-flag.svg" width="30px" />
            </li>
            <li
            v-if="allowedCountries.includes('ES')"
              :class="
                countrySelected == 'ES'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('ES')"
            >
              <img src="@/assets/images/es-flag.svg" width="30px" />
            </li>
            <li
            v-if="allowedCountries.includes('BE')"
              :class="
                countrySelected == 'BE'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('BE')"
            >
              <img src="@/assets/images/be-flag.svg" width="30px" />
            </li>
            <li
            v-if="allowedCountries.includes('NL')"
              :class="
                countrySelected == 'NL'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('NL')"
            >
              <img src="@/assets/images/nl-flag.svg" width="30px" />
            </li>
            <li
            v-if="allowedCountries.includes('LU')"
              :class="
                countrySelected == 'LU'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('LU')"
            >
              <img src="@/assets/images/lu-flag.svg" width="30px" />
            </li>
            <li
            v-if="allowedCountries.includes('DE')"
              :class="
                countrySelected == 'DE'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('DE')"
            >
              <img src="@/assets/images/de-flag.svg" width="30px" />
            </li>
            <li
              v-if="allowedCountries.includes('AT')"
              :class="
                countrySelected == 'AT'
                  ? 'mondial-relay-selector__dropdown--selected'
                  : ''
              "
              @click="selectCountry('AT')"
            >
              <img src="@/assets/images/at-flag.svg" width="30px" />
            </li>
          </ul>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  props: ["countrySelected", "allowedCountries"],
  data() {
    return {
      open: false,
    };
  },
  methods: {
    selectCountry(country) {
      this.$emit("changeCountry", country);
      this.open = false;
    },
  },
};
</script>


<style lang="scss">
.mondial-relay-selector {
  position: relative;

  &__dropdown {
    position: absolute;
    background-color: white;
    width: 100%;
    text-align: center;
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

  &__input {
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

    &__flag {
      position: absolute;
      top: 8px;
      left: 14px;
    }
  }
}
</style>
