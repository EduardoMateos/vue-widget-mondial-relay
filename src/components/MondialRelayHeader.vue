
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
  @media (min-width: 576px) {
    display: flex;
  }
  @media (max-width: 576px) {
    display: row;
  }
  
  text-align: center;

  &__logo {
    width: 64px;
    @media (max-width: 576px) {
      margin: auto;
    }
  }

  &__column {
    display: flex;
    flex-wrap: wrap;
    text-align: center;
    padding: 20px;
    @media (min-width: 576px) {
      flex: 50%;
    }
    &--align-right {
      text-align: right;
    }
  }
}
</style>
