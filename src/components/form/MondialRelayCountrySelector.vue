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
      />
      <div class="mondial-relay-selector__input__flag">
        <img src="./../../assets/images/spain-flag.svg" width="30px" />
      </div>
      <div class="mondial-relay-selector__input__toggle">
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


<style lang="scss">
.mondial-relay-selector {
  position: relative;

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
