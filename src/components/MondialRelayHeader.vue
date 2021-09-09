
<template>
  <div class="mondial-relay-header">
    <div class="mondial-relay-header__column">
      <img
        src="https://widget.mondialrelay.com/parcelshop-picker/v4_0/css/imgs/mr-64.png"
        alt="mondial relay"
        class="mondial-relay-header__logo"
      />
    </div>
    <div
      class="
        mondial-relay-header__column mondial-relay-header__column--align-right
      "
    >
      <slot name="formSearchCp"></slot>
    </div>
  </div>
</template>


<script>
export default {
  props: ["message"],
};
</script>

<style lang="scss">
.mondial-relay-header {
  display: flex;
  flex-wrap: wrap;
  text-align: center;

  &__logo {
    width: 64px;
  }

  &__column {
    display: flex;
    flex-wrap: wrap;
    text-align: center;
    @media (max-width: 576px) {
      flex: 100%;
    }
    @media (min-width: 576px) {
      flex: 50%;
    }
    &--align-right {
      text-align: right;
    }
  }
}
</style>
