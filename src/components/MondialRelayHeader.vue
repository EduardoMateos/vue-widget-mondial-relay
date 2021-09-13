
<template>
  <div class="mondial-relay-header">
    <div class="mondial-relay-header__title">{{ headerTitle }}</div>
    <div class="mondial-relay-header__row">
      <div
        class="mondial-relay-header__column mondial-relay-header__column--left"
      >
        <img
          src="https://widget.mondialrelay.com/parcelshop-picker/v4_0/css/imgs/mr-64.png"
          alt="mondial relay"
          class="mondial-relay-header__logo"
        />
      </div>
      <div
        class="mondial-relay-header__column mondial-relay-header__column--right"
      >
        <slot name="formSearchCp"></slot>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  props: ["headerTitle"],
};
</script>

<style lang="scss">
.mondial-relay-header {
  &__title {
    color: #666;
    text-align: center;
    background: #eee;
    padding: 3px;
    border-bottom: solid 1px #ddd;
  }
  &__row {
    display: grid;
    gap: 1rem;
    grid-template-columns: 50% 50%;
    @media (min-width: 576px) {
      grid-template-columns: 20% 80%;
    }
    @media (max-width: 576px) {
      grid-template-columns: 100%;
    }
  }

  &__column {
    display: flex;
    padding: 8px 20px 8px 20px;
    align-items: center;

    @media (min-width: 576px) {
      &--left {
        justify-content: flex-start;
      }
      &--right {
        justify-content: flex-end;
      }
    }
    @media (max-width: 576px) {
      justify-content: center;
    }
  }

  &__logo {
    width: 64px;
    @media (max-width: 576px) {
      margin: auto;
    }
  }
}
</style>
