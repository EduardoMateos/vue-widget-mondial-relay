<template>
  <div>
    <div class="mondial-relay-group">
      <input
        class="
          mondial-relay-group__input
        "
        id="cp"
        readonly="readonly"
        name="cp"
        required
        size="8"
      />
      <label
        for="cp"
        class="
          mondial-relay-group__input__label
          mondial-relay-group__input__label--top
        "
        >País
      </label>
      <div class="mondial-relay-group__input__flag">
        <img src="./../assets/images/spain-flag.svg" width="30px" />
      </div>
      <div class="mondial-relay-group__input__toggle">
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
    </div>
  </div>
</template>

<script>
export default {
  name: "CountrySelector",
  data() {
    return {
      countries: [
        {
          name: "spain",
        },
        {
          name: "francia",
        },
      ],
      open: true,
    };
  },
};
</script>