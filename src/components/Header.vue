
<template>
  <div class="mondial-relay-header">
    <img
      src="https://widget.mondialrelay.com/parcelshop-picker/v4_0/css/imgs/mr-64.png"
      alt="mondial relay"
    />

    <div class="mondial-relay-form">
      <div class="mondial-relay-form__countries">
        <CountrySelector></CountrySelector>
      </div>
      <div class="mondial-relay-form__cp">
        <div class="mondial-relay-group">
          <input
            class="mondial-relay-group__input"
            id="cp"
            name="cp"
            v-model="cp"
            required
            size="8"
            :class="cp != null && cp.length > 0 ? 'mondial-relay-group__input--has-text' : ''"
          />
          <label
            for="cp"
            class="mondial-relay-group__input__label"
            :class="
              cp != null && cp.length > 0
                ? 'mondial-relay-group__input__label--top'
                : ''
            "
            >Código postal
          </label>
        </div>
      </div>
      <button type="button" v-on:click="search()">Buscar</button>
    </div>
  </div>
</template>


<script>
import CountrySelector from "./CountrySelector";
export default {
  name: "Header",
  components: {
    CountrySelector,
  },
  data() {
    return {
      cp: 28037,
    };
  },
  methods: {
    search() {
      this.$emit("searchByCp", this.cp);
    },
  },
};
</script>

<style lang="scss">
.mondial-relay-header {
  //display: flex;
  flex-wrap: wrap;
  margin: 12px;
}

.mondial-relay-form {
  max-width: 600px;
  &__countries {
    display: inline-block;
    max-width: 100%;
    width: 78px;
  }
  &__cp {
    display: inline-block;
    max-width: 100%;
    width: 128px;
  }
}
</style>
